Packet Number Encryption solves ossification issues caused by middle-boxes creatively using clear text packet number and reduces privacy issues during path migrations. The initial spec is PR #1079, which uses a multi-stage approach. This multi-stage approach arguably causes some performance issues. In this summary, we present the problem, document the proposed alternatives, and propose some steps forward.

## Description and analysis of PR 1079

The packet header contain a Packet Number encoded on 8, 16 or 32 bits, representing the least significant bits of the 64 bit sequence number. According to #1079, encryption proceeds as follow:

1) Format a clear text packet as \<header including PN> and \<payload>.
2) Encrypt the payload using AEAD, using the full sequence number as a nonce, and authenticating the entire header.
3) Encrypt the PN, using part of the encrypted payload as a nonce.
4) Send the packet as \<header including encrypted PN>|\<encrypted payload>|\<AEAD checksum>

Decryption follows the reverses steps:

1) Receive the packet as \<header including encrypted PN>|\<encrypted payload>|\<AEAD checksum>.
2) Decrypt the PN, using part of the encrypted payload as a nonce.
3) Expand the PN to a 64 bit sequence number, using the highest received packet number to provide the missing bits.
4) Decrypt the payload using AEAD, using the full sequence number as a nonce, and authenticating the entire clear text header, including the decrypted PN.

Several issues have been raised about this proposal:

1) The PN encryption consumes as much CPU as encrypting a 16 bit block, and thus adds a bit more than 1% to the CPU cost or encryption/decryption in a software implementation.
2) The process requires two passes, fetching some output of the encryption to seed the PN encryption. This is problematic for hardware implementations that typically don't buffer the output for further access.
3) Including the decrypted PN in the authenticated data requires some extra buffer handling during decryption, and is probably not necessary since the sequence number itself is used as a nonce.
4) For small packet payloads (less than 16 bytes), the per packet encryption and decryption overhead could increase as much as 80% when using packet number encryption.

The 3rd issue could be fixed by a simple change in PR #1079, so we will not further discuss it when reviewing alternative proposals.

## List of alternative proposals

There are three big proposals to resolve the issue, some coming with options:

1) Use an alternative PN encryption that does not require the 2 passes; there are various options on what encryption algorithm to use.
2) Add a nonce to the header, and encrypt the sequence number.
3) Increase the PN size to 64 bits and encrypt it using an 64-bit block cipher.

These procedures have various degrees of benefits and difficulties.

### Alternative PN encryption

The alternative PN encryption would use the following steps:

1) Format a clear text packet as \<header including PN> and \<payload>.
2) Encrypt the payload using AEAD, using the full sequence number as a nonce, and authenticating the first part of the header but not the PN.
3) Encrypt the PN using a block cipher, or a light weight approximation of a block cipher. (Could be parallel with step 2)
4) Send the packet as \<header including encrypted PN>|\<encrypted payload>|\<AEAD checksum>

Decryption follows the reverse steps:

1) Receive the packet as \<header including encrypted PN>|\<encrypted payload>|\<AEAD checksum>.
2) Decrypt the PN using a block cipher.
3) Expand the PN to a 64 bit sequence number, using the highest received packet number to provide the missing bits.
4) Decrypt the payload using AEAD, using the full sequence number as a nonce, and authenticating the entire clear text header but not the decrypted PN.

The advantages of this approach are:

1) Potentially lighter weight encryption, thus potentially less than 1% overhead,
2) Single pass, thus mostly implementable in hardware if we assume that the PN encryption/decryption can be done in software.

The first issue with that approach is that it requires encryption algorithms operating on short blocks, since the PN can be encoded on 8, 16 or 32 bits. Such algorithms are potentially weaker than mainline algorithms like AES or ChaCha20. Some propositions included:

* Simple XOR
* Using a simple obfuscation like the [Fisher Yates shuffle](https://en.wikipedia.org/wiki/Fisher–Yates_shuffle)
* Use the 32-bit cipher [IPCrypt](https://github.com/veorq/ipcrypt) which is also used for [encrypting DNS logs](https://github.com/PowerDNS/ipcipher) 
* Or ask researchers to produce a more robust 32-bit cipher than IPCrypt.

We can argue that the algorithm does not have to be as robust as AES. The main requirement is to prevent real time analysis of sequence numbers by middle-boxes. This is achieved as long as the encryption key cannot be retrieved in a short time by the middle-boxes. If they could do that, we would be once again on the path to ossification. XOR and simple obfuscation probably don't meet that goal, but IPCrypt probably does, and a more robust algorithm certainly would. 

On the other hand, this argument about accepting weak encryption only holds if we relax the requirement to defend against linkability. A pervasive monitor might collect CIDs and encrypted PNs that were being used, use an offline attack to break the encryption, and then use the encrypted PNs as a source to correlate CIDs to reconstruct a QUIC connection. IPcrypt is not really designed to protect against such attacks, but a stronger algorithm might.

The second issue with this approach is that numbers repeat. In the absence of an external nonce, the same clear text PN is always encrypted as the same value. For example, if we used 8-bit numbers, the same sequence of 256 numbers would repeat again and again. This probably means that if we used this simple alternative encryption, we would have to use 32 bit PN, which would be OK as long as the connection does not send more than 2<sup>32</sup> packets -- or maybe fewer, since for example if 2<sup>32</sup>-1 packets have been sent the next number is very predictable. Longer connections would have to rotate the key after about 2<sup>31</sup> packets. This relatively "short rekey interval" is probably the worst problem of this approach.

### Additional nonce

The additional nonce approach assumes that the packet format has been changed, to include an additional nonce in the header. With that approach, encryption would use the following steps:

1) Pick a nonce for the packet and document it in the header;
2) Format the clear text packet as \<header including nonce> and \<pn>|\<payload>.
3) Encrypt the concatenated PN and payload using AEAD, using the nonce in the header as a nonce, and authenticating the first part of the header, but maybe not the nonce.
4) Send the packet as \<header including encrypted nonce>|\<encrypted PN and payload>|\<AEAD checksum>

Decryption follows the reverse steps:

1) Receive the packet as \<header including encrypted nonce>|\<encrypted PN and payload>|\<AEAD checksum>.
2) Decrypt the payload using AEAD, using the nonce from the header, and authenticating the entire clear text header but not the decrypted PN.

The advantage of the approach it its full compatibility with hardware encryption. The issues are the extra overhead of carrying a nonce, and the need to manage the nonce.

Nonce must be chosen so that they do not repeat over the lifetime of the connection, or at least over the life time of the key. We cannot use a simple sequence number, because the middle-boxes would be tempted to use this sequence number for creative algorithms leading to ossification. So we need to guarantee uniqueness in one of two ways:
 
* Due to the birthday paradox, nonce can be statistically unique if at least twice longer than the longest sequence number use for a given set of keys -- 64 bit if sending fewer than 2<sup>32</sup> packets, 128 bits if we want to use the full 64 bit sequence numbers in QUIC.

* Nonce can be guaranteed unique if they are encrypted sequence numbers. For example, encrypting the 64-bit sequence number would produce guaranteed unique nonce for the duration of the connection.

Given that, the "encrypted 64 bit nonce" is the simplest nonce design. It requires using a reasonable 64 bit encryption algorithm. The old generation of 64-bit ciphers would work but is probably sub-optimal. Modern algorithms like
[SPARX](https://www.cryptolux.org/index.php/SPARX) would be preferable.

### 64bit encrypted PN

If we assume that a reasonable 64 bit cipher is available, we can improve on the "additional nonce" approach by simply
sending 64 bit encrypted sequence numbers. The approach assumes that the packet format is changed to always encode the PN on 64 bits.With that approach, encryption would use the following steps:

1) Encrypt the 64 bit sequence number.
2) Format the packet as \<header including encrypted-PN>|\<payload>.
3) Encrypt the concatenated PN and payload using AEAD, using the encrypted-PN as a nonce, and authenticating the first part of the header, but maybe not the encrypted-PN.
4) Send the packet as \<header including encrypted-PN>|\<encrypted payload>|\<AEAD checksum>

Decryption follows the reverse steps:

1) Receive the packet as \<header including encrypted PN>|\<payload>|\<AEAD checksum>.
2) Decrypt the payload using AEAD, using the encrypted PN from the header, and authenticating the entire clear text header but not the encrypted PN.
3) Decrypt the encrypted PN to obtain the sequence number.

Like the nonce approach, this approach is well compatible with hardware encryption. It generates fewer byte overhead than the nonce approach, since the PN does not have to be repeated, although requiring 8 bytes for the PN is arguably 4 to 7 bytes more than the encodings on 4, 2 or 1 octet. The only performance issue left is the assumption that PN encryption or decryption is performed in software, which is probably necessary as long as we don't have hardware specialized for QUIC.

The solution depends on a plausible 64-bit block cipher, such as for example [SPARX](https://www.cryptolux.org/index.php/SPARX), but we can apply the same qualification as when analyzing the alternative encryption approach. If we relax the requirement to protect against linkability, the algorithm does not need to be "fully unbreakable", it just need to be a sufficient deterrent against meddling by middle-boxes. On the other hand, according to its designers, the SPARX cipher may well be strong enough.

## Comparing the different solutions

After reviewing the different solutions, we can draw a comparison base on the following criteria:

 * Defense against ossification
 * Defense against linkability
 * Support for hardware encryption
 * Transmission overhead
 * Software overhead
 * Cryptographic agility
 * Requires frequent rekeying

The (#alternative-pn-encryption) and (#64bit-encrypted-pn) solutions rely on special purpose algorithms,
which raises questions about crypto agility. #1179 has agility in sense that the corresponding CTR mode is used for encrypting the PN. OTOH, the two alternatives using a simple cipher are not agile. So we might need to define two types of simpler ciphers for PN encryption, if consider that privacy issues (like the one above) might arise in the future. These two alternatives will require adding a PN cryptographic algorithm negotiation to the handshake.

Solution that use breakable algorithms do not provide linkability defense against a determined adversary. This is obvious for variations of (#alternative-pn-encryption) and (#64bit-encrypted-pn) that would use weak 32-bit or 64-bit cipher. There is a similar issue with the (#additional-nonce) approach, if the nonce is generated by encrypting a sequence number. If the encryption is weak, the adversaries can decrypt the nonce and use it for linkability in the same way as a sequence number. The cost of not defending against linkability is software complexity: each path will have to be managed as a separate connection, with separate encryption keys, independent sequence numbers, and independent management of acknowledgements.

Here is the comparison table:

|         | Ossify| Link? | HW| bytes overhead | CPU overhead | Crypto agile | Requires Rekeying |
|--------------|--------------|-------------|----------|----------------|--------------|--------------|--------------|
| PR #1079| OK | OK| Hard | 0 | 1% | Yes | No |
||||||||
| Alternative PN ||||||||
| (obfuscation)   | NO | NO| OK | 0 to 3 | \<\< 1% | N/A | Yes |
| (weak algo)   | OK| NO| OK | 0 to 3 | \< 1% | NO? | Yes |
| (strong algo)   | OK| OK| OK | 0 to 3 | \< 1% | NO? | Yes |
|||||||
| Additional nonce |||||||
| (random bytes) | OK| OK| OK | 16 | \< 1% | Yes | No |
| (CTR encrypt) | Maybe | OK| OK | 8 | \< 1% | Yes | No |
|||||||
| 64bit-encrypted-pn  |||||||
| (weak algo)   |  NO | OK| OK | 4 to 7 | \< 1% | NO? | No |
| (strong algo)  | OK| OK| OK | 4 to 7 | \< 1% | NO? | No |
