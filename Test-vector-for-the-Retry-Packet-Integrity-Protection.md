**OOPS. Lucy moved the football again. The values below do not match the latest draft, don't use them!**


The Retry Packet Integrity protection is defined in section 5.8 of the TLS draft. It requires authenticating an augmented version of the Retry Packet with AEAD GCM128, for which:
* The secret key, K, is 128 bits equal to 0xf5ed4642e0e4c8d878bbbc8a828821c9.
* The nonce, N, is 96 bits all set to zero.
We will test a Retry Packet with the following parameters:
```
#define RETRY_PROTECTION_FIRST_BYTE 0xF5
#define RETRY_PROTECTION_VERSION 0xFF,0,0,25
#define RETRY_PROTECTION_TEST_DCID_LENGTH 6
#define RETRY_PROTECTION_TEST_DCID_BYTES 61,62,63,64,65,66
#define RETRY_PROTECTION_TEST_SCID_LENGTH 4
#define RETRY_PROTECTION_TEST_SCID_BYTES 44,45,46,47
#define RETRY_PROTECTION_TEST_RETRY_TOKEN_LENGTH
#define RETRY_PROTECTION_TEST_RETRY_TOKEN \
    101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116, \
    117,118,119,120,121,122,123,124
```
The Retry packet is thus:
```
uint8_t retry_protection_test_input[] = {
    RETRY_PROTECTION_FIRST_BYTE,
    RETRY_PROTECTION_VERSION,
    RETRY_PROTECTION_TEST_DCID_LENGTH,
    RETRY_PROTECTION_TEST_DCID_BYTES,
    RETRY_PROTECTION_TEST_SCID_LENGTH,
    RETRY_PROTECTION_TEST_SCID_BYTES,
    RETRY_PROTECTION_TEST_RETRY_TOKEN
};
```
According to the spec, we construct first the Retry Pseudo-Packet by prepending the following ODCID:
```
#define RETRY_PROTECTION_TEST_ODCID_LENGTH 8
#define RETRY_PROTECTION_TEST_ODCID_BYTES 81,82,83,84,85,86,87,88
```
The  Retry Pseudo-Packet is:
```
uint8_t retry_protection_pseudo_packet[] = {
    RETRY_PROTECTION_TEST_ODCID_LENGTH,
    RETRY_PROTECTION_TEST_ODCID_BYTES,
    RETRY_PROTECTION_FIRST_BYTE,
    RETRY_PROTECTION_VERSION,
    RETRY_PROTECTION_TEST_DCID_LENGTH,
    RETRY_PROTECTION_TEST_DCID_BYTES,
    RETRY_PROTECTION_TEST_SCID_LENGTH,
    RETRY_PROTECTION_TEST_SCID_BYTES,
    RETRY_PROTECTION_TEST_RETRY_TOKEN
};
```
We run AEAD encryption with the specified key and nonce, on an empty plain text and using the pseudo-packet as authenticated data. The result should be:
```
uint8_t retry_integrity_checksum[16] = {
    0x53, 0x1d, 0xb4, 0xc5, 0x88, 0x7a, 0x3a, 0xb0,
    0x7a, 0xa4, 0xfa, 0x35, 0xc2, 0xfb, 0xa6, 0x95 };
```