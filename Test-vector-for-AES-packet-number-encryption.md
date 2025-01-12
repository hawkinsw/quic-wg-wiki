Better test this early, so we don't have problems when doing interop tests.

Let's assume that the packet's payload is encrypted using AES GCM 128.

Let's also assume that the PN encryption key is:
~~~
    static const uint8_t key[] = {
        0x2b, 0x7e, 0x15, 0x16, 0x28, 0xae, 0xd2, 0xa6,
        0xab, 0xf7, 0x15, 0x88, 0x09, 0xcf, 0x4f, 0x3c };
~~~
And that the packet received from the network is:
~~~
    static const uint8_t packet_encrypted_pn[] = {
        0x30,
        0x80, 0x6d, 0xbb, 0xb5,
        0x6b, 0xc1, 0xbe, 0xe2, 0x2e, 0x40, 0x9f, 0x96,
        0xe9, 0x3d, 0x7e, 0x11, 0x73, 0x93, 0x17, 0x2a,
        0x20, 0x3f, 0xbe, 0x2e, 0x32, 0x17, 0xfc, 0x5b,
        0x88, 0x55
    };
~~~
The first byte indicates that it is a short header. This connection does not use connection
IDs.

The PN value in the packet is encrypted. From the packet, we can extract the sample:
~~~
    static const uint8_t sample[] = {
        0x6b, 0xc1, 0xbe, 0xe2, 0x2e, 0x40, 0x9f, 0x96,
        0xe9, 0x3d, 0x7e, 0x11, 0x73, 0x93, 0x17, 0x2a };
~~~
Using that sample to construct IV as specified, the PN should decrypt to:
~~~
   static const uint8_t clear_pn[] = {
        0xba, 0xba, 0xc0, 0x01
    };
~~~

As `(clear_pn[0] >> 6) == 2`, the actual length is 2 (see [Table 2: Packet Number Encodings for Packet Headers](https://tools.ietf.org/html/draft-ietf-quic-transport-12#page-19)). Therefore `0xc0, 0x01` must be ignored and the actual packet number is `0xbaba`.