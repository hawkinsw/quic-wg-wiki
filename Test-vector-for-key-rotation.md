The key rotation operates with a call to HKDF, using the label `traffic upd` and the prefix `tls13 ` as specified in section 7.2 of [RFC8446](https://datatracker.ietf.org/doc/rfc8446/?include_text=1). The transform depends on the negotiated cipher suite, and specifically on the hash function specified in that suite.

Here are 2 sets of test vectors used by Picoquic. 

## Suite aes256 gcm sha384

Initial secret:
```
static const uint8_t key_rotation_test_init_sha384[] = {
    1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,
    17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32,
    33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48};
```
Rotated secret:
```
static const uint8_t key_rotation_test_target_sha384[] = {
    0x9e, 0x4b, 0xc4, 0x52, 0xee, 0xb9, 0xe7, 0x49,
    0x91, 0x09, 0x41, 0x44, 0x87, 0xd6, 0xda, 0x7c,
    0xe8, 0xdb, 0x5b, 0x1a, 0xa7, 0x5c, 0x7e, 0xf6,
    0x30, 0x08, 0xed, 0xb4, 0x4e, 0x0d, 0xfb, 0xe3,
    0xf7, 0xe1, 0xd8, 0x68, 0x9e, 0x00, 0xd4, 0xe5,
    0x85, 0x9b, 0xb7, 0x84, 0xf6, 0x37, 0xfb, 0x74};
```
## Suite aes128 gcm sha256

Initial secret:
```
static const uint8_t key_rotation_test_init_sha256[] = {
    1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,
    17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32};
```
Rotated secret:
```
static const uint8_t key_rotation_test_target_sha256[] = { 
    0xeb, 0xf1, 0xd6, 0xd4, 0xaf, 0xa5, 0xf7, 0xdd,
    0xdc, 0x88, 0xb3, 0x68, 0x31, 0xee, 0xb3, 0xbc,
    0x06, 0x44, 0x2d, 0xd6, 0x89, 0xb1, 0x78, 0x26,
    0x8a, 0xba, 0x45, 0xa0, 0x2b, 0xd8, 0x1f, 0x07};
```