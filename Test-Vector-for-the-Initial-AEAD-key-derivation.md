In the early drafts, the clear text messages of Quic were protected by an FNV1A
checksum. Draft-07 replaces that by AEAD encryption, in which the keys are derived
from a version specific salt combined with the initial connection ID. Then draft-09
changed some of tags associated with the derivation, draft-10 both introduced
a new salt and changed the key expansion process, draft-14 changed the labels, and,
finally, draft-17 changed the salt, the KDF function, and the labels.

Early interop tests of draft-07 showed that a test vector for the key derivation
would be useful. They still are for the new versions. They were updated for
draft-09, draft-10, draft-14, draft-17, and now draft-21.

# draft-21 Test Vectors
## Connection parameters:

The connection ID used in the test vector is:
~~~
0xc654efd8a31b4792
~~~
This version number corresponds to draft-21, which says:
~~~

   initial_salt = 0x7fbcdb0e7c66bbe9193a96cd21519ebd7a02644a
   initial_secret = HKDF-Extract(initial_salt,
                                 client_dst_connection_id)

   client_initial_secret = HKDF-Expand-Label(initial_secret,
                                             "client in", "",
                                             Hash.length)
   server_initial_secret = HKDF-Expand-Label(initial_secret,
                                             "server in", "",
                                             Hash.length)
~~~
## Computation of the cleartext secret
The parameters to the computation are the `initial_salt`, and the 
initial connection ID, serialized as 8 bytes:
~~~
        0xc6, 0x54, 0xef, 0xd8, 0xa3, 0x1b, 0x47, 0x92
~~~
The 32-byte initial secret will be:
~~~
        0x23, 0x37, 0x55, 0x67, 0xbc, 0x4f, 0xe8, 0x14,
        0x22, 0x59, 0x7f, 0xbf, 0x24, 0xb3, 0x0a, 0x01,
        0x3c, 0xb1, 0x24, 0xae, 0x37, 0x09, 0x4d, 0xf7,
        0x1c, 0xbb, 0xd7, 0x98, 0xff, 0x24, 0x33, 0xb6
~~~
## Computation of the client clear text secret:

The label used as parameter to QHKDF expand the clear text secret and produce the
client secret is 19 bytes long:
~~~
        0x00, 0x20, 0x0f, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x63, 0x6c, 0x69, 0x65, 0x6e, 0x74, 0x20,
        0x69, 0x6e, 0x00,
~~~
The 32 bytes client initial secret will be:
~~~
        0xf3, 0x30, 0x76, 0x33, 0x57, 0xe7, 0x8b, 0xa3,
        0xc9, 0x48, 0xa5, 0xbb, 0xbe, 0x28, 0xaa, 0x2a,
        0x38, 0x6c, 0x10, 0xc7, 0xf4, 0x32, 0xd8, 0x97,
        0xa7, 0x7e, 0x2b, 0x24, 0x4b, 0x03, 0x05, 0x33
~~~
This will produce the following key, IV, and HP key:
~~~
Client AEAD Key (16 bytes):
        0xd4, 0xe4, 0x3d, 0x22, 0x68, 0xf8, 0xe4, 0x3b,
        0xab, 0x1c, 0xa6, 0x7a, 0x36, 0x80, 0x46, 0x0f

Client AEAD IV (12 bytes):
        0x67, 0x1f, 0x1c, 0x3d, 0x21, 0xde, 0x47, 0xff,
        0x01, 0x8b, 0x11, 0x3b

Client AEAD HP Key (16 bytes):
        0xed, 0x6c, 0x63, 0x14, 0xdd, 0xc8, 0x69, 0xa5,
        0x94, 0x19, 0x74, 0x42, 0x87, 0x71, 0x39, 0x83
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the handshake secret and produce the
server secret is 19 bytes long:
~~~
        0x00, 0x20, 0x0f, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x73, 0x65, 0x72, 0x76, 0x65, 0x72, 0x20,
        0x69, 0x6e, 0x00,
~~~
The 32 bytes server initial secret will be:
~~~
        0x5e, 0x84, 0x2c, 0xcb, 0x6c, 0xac, 0x42, 0xa7,
        0x22, 0x48, 0xbd, 0x57, 0xd3, 0x30, 0x64, 0x65,
        0xd3, 0xde, 0x66, 0x50, 0x64, 0x1e, 0x1f, 0xb1,
        0x34, 0xdc, 0x87, 0xf5, 0x4a, 0xc8, 0xad, 0x74
~~~
This will produce the following key, IV, and HP key:
~~~
Server AEAD key (16 bytes):
        0x9d, 0xa3, 0x3b, 0xa0, 0x27, 0x46, 0xa3, 0xd3,
        0x58, 0x12, 0x89, 0xc0, 0x19, 0x9c, 0x3a, 0xf2,

Server AEAD IV (12 bytes):
        0xe6, 0x9c, 0x4e, 0xaf, 0xce, 0x11, 0x3d, 0xb5,
        0x70, 0xb9, 0x4c, 0x0c

Server AEAD HP Key (16 bytes):
        0xc5, 0x0f, 0x34, 0x99, 0x5b, 0x8a, 0xa7, 0x16,
        0x08, 0x7b, 0x64, 0x87, 0x6e, 0xdd, 0x68, 0x38
~~~
# draft-17 Test Vectors
## Connection parameters:

The connection ID used in the test vector is:

~~~
0xc654efd8a31b4792
~~~
This version number corresponds to draft-17, which says:
~~~

   initial_salt = 0xef4fb0abb47470c41befcf8031334fae485e09a0
   initial_secret = HKDF-Extract(initial_salt,
                                 client_dst_connection_id)

   client_initial_secret = HKDF-Expand-Label(initial_secret,
                                             "client in", "",
                                             Hash.length)
   server_initial_secret = HKDF-Expand-Label(initial_secret,
                                             "server in", "",
                                             Hash.length)
~~~
## Computation of the cleartext secret
The parameters to the computation are the `initial_salt`, and the 
initial connection ID, serialized as 8 bytes:
~~~
        0xc6, 0x54, 0xef, 0xd8, 0xa3, 0x1b, 0x47, 0x92
~~~
The 32-byte handshake secret will be:
~~~
        0x5f, 0x8d, 0xa5, 0x94, 0xfe, 0xca, 0x72, 0xc1,
        0x0f, 0x9e, 0xc8, 0x78, 0x81, 0x11, 0x05, 0x57,
        0x81, 0xa9, 0x6f, 0x6a, 0x06, 0x53, 0x58, 0xbf,
        0xb4, 0x5a, 0xba, 0x4b, 0xc0, 0x37, 0xf3, 0xb2,
~~~
## Computation of the client clear text secret:

The label used as parameter to QHKDF expand the clear text secret and produce the
client secret is 19 bytes long:
~~~
        0x00, 0x20, 0x0f, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x63, 0x6c, 0x69, 0x65, 0x6e, 0x74, 0x20,
        0x69, 0x6e, 0x00,
~~~
The 32 bytes client clear text secret will be:
~~~
        0x0c, 0x74, 0xbb, 0x95, 0xa1, 0x04, 0x8e, 0x52,
        0xef, 0x3b, 0x72, 0xe1, 0x28, 0x89, 0x35, 0x1c,
        0xd7, 0x3a, 0x55, 0x0f, 0xb6, 0x2c, 0x4b, 0xb0,
        0x87, 0xe9, 0x15, 0xcc, 0xe9, 0x6c, 0xe3, 0xa0
~~~
This will produce the following key, IV, and HP key:
~~~
Client AEAD Key (16 bytes):
        0x86, 0xd1, 0x83, 0x04, 0x80, 0xb4, 0x0f, 0x86,
        0xcf, 0x9d, 0x68, 0xdc, 0xad, 0xf3, 0x5d, 0xfe,

Client AEAD IV (12 bytes):
        0x12, 0xf3, 0x93, 0x8a, 0xca, 0x34, 0xaa, 0x02,
        0x54, 0x31, 0x63, 0xd4,

Client AEAD HP Key (16 bytes):
        0xcd, 0x25, 0x3a, 0x36, 0xff, 0x93, 0x93, 0x7c,
        0x46, 0x93, 0x84, 0xa8, 0x23, 0xaf, 0x6c, 0x56,
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the handshake secret and produce the
server secret is 19 bytes long:
~~~
        0x00, 0x20, 0x0f, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x73, 0x65, 0x72, 0x76, 0x65, 0x72, 0x20,
        0x69, 0x6e, 0x00,
~~~
The 32 bytes server handshake secret will be:
~~~
        0x4c, 0x9e, 0xdf, 0x24, 0xb0, 0xe5, 0xe5, 0x06,
        0xdd, 0x3b, 0xfa, 0x4e, 0x0a, 0x03, 0x11, 0xe8,
        0xc4, 0x1f, 0x35, 0x42, 0x73, 0xd8, 0xcb, 0x49,
        0xdd, 0xd8, 0x46, 0x41, 0x38, 0xd4, 0x7e, 0xc6,
~~~
This will produce the following key, IV, and HP key:
~~~
Server AEAD key (16 bytes):
        0x2c, 0x78, 0x63, 0x3e, 0x20, 0x6e, 0x99, 0xad,
        0x25, 0x19, 0x64, 0xf1, 0x9f, 0x6d, 0xcd, 0x6d,

Server AEAD IV (12 bytes):
        0x7b, 0x50, 0xbf, 0x36, 0x98, 0xa0, 0x6d, 0xfa,
        0xbf, 0x75, 0xf2, 0x87,

Server AEAD HP Key (16 bytes):
        0x25, 0x79, 0xd8, 0x69, 0x6f, 0x85, 0xed, 0xa6,
        0x8d, 0x35, 0x02, 0xb6, 0x55, 0x96, 0x58, 0x6b,
~~~


# draft-14 Test Vectors
## Connection parameters:

The connection ID and version number used in the test vector are:
~~~
Initial connection ID: 0x8394c8f03e515708
Version:               0xff00000e
~~~
This version number corresponds to draft-14, which says:
~~~
    handshake_salt = 0x9c108f98520a5c5c32968e950e8a2c5fe06d6c38

    handshake_secret = HKDF-Extract(quic_version_1_salt,
                                    client_connection_id)

    client_handshake_secret =
                       QHKDF-Expand(handshake_secret, "client in",
                                    Hash.length)
    server_handshake_secret =
                       QHKDF-Expand(handshake_secret, "server in",
                                    Hash.length)
~~~
## Computation of the cleartext secret

The parameters to the computation are the quic_version1_salt, and the 
initial connection ID, serialized as 8 bytes:
~~~
	0x83, 0x94, 0xc8, 0xf0, 0x3e, 0x51, 0x57, 0x08
~~~
The 32-byte handshake secret will be:
~~~
        0xa5, 0x72, 0xb0, 0x24, 0x5a, 0xf1, 0xed, 0xdf, 
        0x5c, 0x61, 0xc6, 0xe3, 0xf7, 0xf9, 0x30, 0x4c, 
        0xa6, 0x6b, 0xfb, 0x4c, 0xaa, 0xf7, 0x65, 0x67, 
        0xd5, 0xcb, 0x8d, 0xd1, 0xdc, 0x4e, 0x82, 0x0b
~~~
## Computation of the client clear text secret:

The label used as parameter to QHKDF expand the clear text secret and produce the
client secret is 18 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x71, 0x75, 0x69, 0x63, 0x20,
        0x63, 0x6c, 0x69, 0x65, 0x6e, 0x74, 0x20, 0x69,
        0x6e, 0x00,
~~~
The 32 bytes client clear text secret will be:
~~~
        0x9f, 0x53, 0x64, 0x57, 0xf3, 0x2a, 0x1e, 0x0a,
        0xe8, 0x64, 0xbc, 0xb3, 0xca, 0xf1, 0x23, 0x51,
        0x10, 0x63, 0x0e, 0x1d, 0x1f, 0xb3, 0x38, 0x35,
        0xbd, 0x05, 0x41, 0x70, 0xf9, 0x9b, 0xf7, 0xdc,
~~~
This will produce the following key, IV, and PN key:
~~~
Client AEAD Key (16 bytes):
        0xf2, 0x92, 0x8f, 0x26, 0x14, 0xad, 0x6c, 0x20,
        0xb9, 0xbd, 0x00, 0x8e, 0x9c, 0x89, 0x63, 0x1c,

Client AEAD IV (12 bytes):
        0xab, 0x95, 0x0b, 0x01, 0x98, 0x63, 0x79, 0x78,
        0xcf, 0x44, 0xaa, 0xb9,

Client AEAD PN Key (16 bytes):
        0x68, 0xc3, 0xf6, 0x4e, 0x2d, 0x66, 0x34, 0x41,
        0x2b, 0x8e, 0x32, 0x94, 0x62, 0x8d, 0x76, 0xf1
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the handshake secret and produce the
server secret is 18 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x71, 0x75, 0x69, 0x63, 0x20,
        0x73, 0x65, 0x72, 0x76, 0x65, 0x72, 0x20, 0x69,
        0x6e, 0x00,
~~~
The 32 bytes server handshake secret will be:
~~~
        0xb0, 0x87, 0xdc, 0xd7, 0x47, 0x8d, 0xda, 0x8a,
        0x85, 0x8f, 0xbf, 0x3d, 0x60, 0x5c, 0x88, 0x85,
        0x86, 0xc0, 0xa3, 0xa9, 0x87, 0x54, 0x23, 0xad,
        0x4f, 0x11, 0x4f, 0x0b, 0xa3, 0x8e, 0x5a, 0x2e,
~~~
This will produce the following key, IV, and PN key:
~~~
Server AEAD key (16 bytes):
        0xf5, 0x68, 0x17, 0xd0, 0xfc, 0x59, 0x5c, 0xfc,
        0x0a, 0x2b, 0x0b, 0xcf, 0xb1, 0x87, 0x35, 0xec,

Server AEAD IV (12 bytes):
        0x32, 0x05, 0x03, 0x5a, 0x3c, 0x93, 0x7c, 0x90,
        0x2e, 0xe4, 0xf4, 0xd6,

Server AEAD PN Key (16 bytes):
        0xa3, 0x13, 0xc8, 0x6d, 0x13, 0x73, 0xec, 0xbc,
        0xcb, 0x32, 0x94, 0xb1, 0x49, 0x74, 0x22, 0x6c
~~~

# draft-10 Test Vectors
## Connection parameters:

The connection ID and version number used in the test vector are:
~~~
Initial connection ID: 0x8394c8f03e515708
Version:               0xff00000a
~~~
This version number corresponds to draft-10, which says:
~~~
    handshake_salt = 0x9c108f98520a5c5c32968e950e8a2c5fe06d6c38

    handshake_secret = HKDF-Extract(quic_version_1_salt,
                                    client_connection_id)

    client_handshake_secret =
                       QHKDF-Expand(handshake_secret, "client hs",
                                    Hash.length)
    server_handshake_secret =
                       QHKDF-Expand(handshake_secret, "server hs",
                                    Hash.length)
~~~
## Computation of the cleartext secret

The parameters to the computation are the quic_version1_salt, and the 
initial connection ID, serialized as 8 bytes:
~~~
	0x83, 0x94, 0xc8, 0xf0, 0x3e, 0x51, 0x57, 0x08
~~~
The 32-byte handshake secret will be:
~~~
        0xa5, 0x72, 0xb0, 0x24, 0x5a, 0xf1, 0xed, 0xdf, 
        0x5c, 0x61, 0xc6, 0xe3, 0xf7, 0xf9, 0x30, 0x4c, 
        0xa6, 0x6b, 0xfb, 0x4c, 0xaa, 0xf7, 0x65, 0x67, 
        0xd5, 0xcb, 0x8d, 0xd1, 0xdc, 0x4e, 0x82, 0x0b
~~~
## Computation of the client clear text secret:

The label used as parameter to QHKDF expand the clear text secret and produce the
client secret is 17 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x51, 0x55, 0x49, 0x43, 0x20,
        0x63, 0x6c, 0x69, 0x65, 0x6e, 0x74, 0x20, 0x68,
        0x73
~~~
The 32 bytes client clear text secret will be:
~~~
        0x83, 0x55, 0xf2, 0x1a, 0x3d, 0x8f, 0x83, 0xec,
        0xb3, 0xd0, 0xf9, 0x71, 0x08, 0xd3, 0xf9, 0x5e,
        0x0f, 0x65, 0xb4, 0xd8, 0xae, 0x88, 0xa0, 0x61,
        0x1e, 0xe4, 0x9d, 0xb0, 0xb5, 0x23, 0x59, 0x1d
~~~
This will produce the following key and IV:
~~~
Client AEAD Key (16 bytes):
        0x3a, 0xd0, 0x54, 0x2c, 0x4a, 0x85, 0x84, 0x74, 
        0x00, 0x63, 0x04, 0x9e, 0x3b, 0x3c, 0xaa, 0xb2

Client AEAD IV (12 bytes):
        0xd1, 0xfd, 0x26, 0x05, 0x42, 0x75, 0x3a, 0xba, 
        0x38, 0x58, 0x9b, 0xad
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the handshake secret and produce the
server secret is 17 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x51, 0x55, 0x49, 0x43, 0x20,
        0x73, 0x65, 0x72, 0x76, 0x65, 0x72, 0x20, 0x68,
        0x73 
~~~
The 32 bytes server handshake secret will be:
~~~
        0xf8, 0x0e, 0x57, 0x71, 0x48, 0x4b, 0x21, 0xcd, 
        0xeb, 0xb5, 0xaf, 0xe0, 0xa2, 0x56, 0xa3, 0x17, 
        0x41, 0xef, 0xe2, 0xb5, 0xc6, 0xb6, 0x17, 0xba,
        0xe1, 0xb2, 0xf1, 0x5a, 0x83, 0x04, 0x83, 0xd6, 
~~~
This will produce the following key and IV:
~~~
Server AEAD key (16 bytes):
        0xbe, 0xe4, 0xc2, 0x4d, 0x2a, 0xf1, 0x33, 0x80, 
        0xa9, 0xfa, 0x24, 0xa5, 0xe2, 0xba, 0x2c, 0xff

Server IV (12 bytes):
        0x25, 0xb5, 0x8e, 0x24, 0x6d, 0x9e, 0x7d, 0x5f, 
        0xfe, 0x43, 0x23, 0xfe
~~~

# draft-09 Test Vectors
## Connection parameters:

The connection ID and version number used in the test vector are:
~~~
Initial connection ID: 0x8394c8f03e515708
Version:               0xff000009
~~~
This version number corresponds to draft-09, which says:
~~~
    quic_version_1_salt = afc824ec5fc77eca1e9d36f37fb2d46518c36639

    handshake_secret = HKDF-Extract(quic_version_1_salt,
                                    client_connection_id)

    client_handshake_secret =
                       QHKDF-Expand(handshake_secret, "client hs",
                                    Hash.length)
    server_handshake_secret =
                       QHKDF-Expand(handshake_secret, "server hs",
                                    Hash.length)
~~~
## Computation of the cleartext secret

The parameters to the computation are the quic_version1_salt, and the 
initial connection ID, serialized as 8 bytes:
~~~
	0x83, 0x94, 0xc8, 0xf0, 0x3e, 0x51, 0x57, 0x08
~~~
The 32-byte handshake secret will be:
~~~
        0x8f, 0x01, 0x00, 0x67, 0x9c, 0x96, 0x5a, 0xc5,
        0x9f, 0x28, 0x3a, 0x02, 0x52, 0x2a, 0x6e, 0x43,
        0xcf, 0xae, 0xf6, 0x3c, 0x45, 0x48, 0xb0, 0xa6,
        0x8f, 0x91, 0x91, 0x40, 0xee, 0x7d, 0x9a, 0x48
~~~
## Computation of the client clear text secret:

The label used as parameter to HKDF expand the clear text secret and produce the
client secret is 18 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x51, 0x55, 0x49, 0x43, 0x20,
        0x63, 0x6c, 0x69, 0x65, 0x6e, 0x74, 0x20, 0x68,
        0x73, 0x00
~~~
The 32 bytes client clear text secret will be:
~~~
        0x8e, 0x28, 0x6a, 0x27, 0x38, 0xe6, 0x66, 0x50,
        0xb4, 0xf8, 0x8f, 0xac, 0x5d, 0xc5, 0xd0, 0xef,
        0x7d, 0x36, 0x9b, 0x07, 0xd4, 0x74, 0x42, 0x99,
        0x1a, 0x00, 0x0c, 0x55, 0xac, 0xc4, 0x0c, 0xf4
~~~
This will produce the following key and IV:
~~~
Client AEAD Key (16 bytes):
        0x6b, 0x6a, 0xbc, 0x50, 0xf7, 0xac, 0x46, 0xd1,
        0x10, 0x8c, 0x19, 0xcc, 0x63, 0x64, 0xbd, 0xe3

Client AEAD IV (12 bytes):
        0xb1, 0xf9, 0xa7, 0xe2, 0x7c, 0xc2, 0x33, 0xbb,
        0x99, 0xe2, 0x03, 0x71
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the handshake secret and produce the
server secret is 18 bytes long:
~~~
        0x00, 0x20, 0x0e, 0x51, 0x55, 0x49, 0x43, 0x20,
        0x73, 0x65, 0x72, 0x76, 0x65, 0x72, 0x20, 0x68,
        0x73, 0x00 
~~~
The 32 bytes server handshake secret will be:
~~~
        0xfa, 0xb5, 0xb7, 0xf5, 0x26, 0xec, 0xaf, 0xaf,
        0x74, 0x71, 0x52, 0xdd, 0xaa, 0x88, 0x28, 0x56,
        0xf9, 0xbe, 0xd7, 0x48, 0x81, 0x1e, 0x37, 0xff,
        0xe1, 0xcb, 0xb1, 0x55, 0xe1, 0xc9, 0x91, 0xad        
~~~
This will produce the following key and IV:
~~~
Server AEAD key (16 bytes):
        0x9e, 0xe7, 0xe8, 0x57, 0x72, 0x00, 0x59, 0xaf,
        0x30, 0x11, 0xfb, 0x26, 0xe1, 0x21, 0x42, 0xc9

Server IV (12 bytes):
        0xd5, 0xee, 0xe8, 0xb5, 0x7c, 0x9e, 0xc7, 0xc4,
        0xbe, 0x98, 0x4a, 0xa5
~~~

# draft-07 Test Vectors
## Connection parameters:

The connection ID and version number used in the test vector are:
~~~
Initial connection ID: 0x8394c8f03e515708
Version:               0xff000007
~~~
This version number corresponds to draft-07, which says:
~~~
    quic_version_1_salt = afc824ec5fc77eca1e9d36f37fb2d46518c36639

    cleartext_secret = HKDF-Extract(quic_version_1_salt,
                                    client_connection_id)

    client_cleartext_secret =
                       HKDF-Expand-Label(cleartext_secret,
                                         "QUIC client cleartext Secret",
                                         "", Hash.length)
    server_cleartext_secret =
                       HKDF-Expand-Label(cleartext_secret,
                                         "QUIC server cleartext Secret",
                                         "", Hash.length)
~~~
## Computation of the cleartext secret

The parameters to the computation are the quic_version1_salt, and the 
initial connection ID, serialized as 8 bytes:
~~~
	0x83, 0x94, 0xc8, 0xf0, 0x3e, 0x51, 0x57, 0x08
~~~
The 32 bytes clear text secret will be:
~~~
        0x8f, 0x01, 0x00, 0x67, 0x9c, 0x96, 0x5a, 0xc5,
        0x9f, 0x28, 0x3a, 0x02, 0x52, 0x2a, 0x6e, 0x43,
        0xcf, 0xae, 0xf6, 0x3c, 0x45, 0x48, 0xb0, 0xa6,
        0x8f, 0x91, 0x91, 0x40, 0xee, 0x7d, 0x9a, 0x48
~~~
## Computation of the client clear text secret:

The label used as parameter to HKDF expand the clear text secret and produce the
client secret is 38 bytes long:
~~~
        0x00, 0x20, 0x22, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x51, 0x55, 0x49, 0x43, 0x20, 0x63, 0x6c,
        0x69, 0x65, 0x6e, 0x74, 0x20, 0x63, 0x6c, 0x65,
        0x61, 0x72, 0x74, 0x65, 0x78, 0x74, 0x20, 0x53,
        0x65, 0x63, 0x72, 0x65, 0x74, 0x00
~~~
The 32 bytes client clear text secret will be:
~~~
        0x31, 0xba, 0x96, 0x68, 0x73, 0xf7, 0xf4, 0x53,
        0xe6, 0xc8, 0xa1, 0xbf, 0x78, 0xed, 0x70, 0x13,
        0xfa, 0xd8, 0x3f, 0xfc, 0xee, 0xfc, 0x95, 0x68,
        0x81, 0xcd, 0x24, 0x1c, 0x0a, 0xe3, 0xa7, 0xa6
~~~
This will produce the following key and IV:
~~~
Client AEAD Key (16 bytes):
        0x2e, 0xbd, 0x78, 0x00, 0xdb, 0xed, 0x20, 0x10,
        0xe5, 0xa2, 0x1c, 0x4a, 0xd2, 0x4b, 0x4e, 0xc3

Client AEAD IV (12 bytes):
        0x55, 0x44, 0x0d, 0x5f, 0xf7, 0x50, 0x3d, 0xe4,
        0x99, 0x7b, 0xfd, 0x6b
~~~
## Computation of the server clear text secret:

The label used as parameter to HKDF expand the clear text secret and produce the
server secret is 38 bytes long:
~~~
        0x00, 0x20, 0x22, 0x74, 0x6c, 0x73, 0x31, 0x33,
        0x20, 0x51, 0x55, 0x49, 0x43, 0x20, 0x73, 0x65,
        0x72, 0x76, 0x65, 0x72, 0x20, 0x63, 0x6c, 0x65,
        0x61, 0x72, 0x74, 0x65, 0x78, 0x74, 0x20, 0x53,
        0x65, 0x63, 0x72, 0x65, 0x74, 0x00
~~~
The 32 bytes server clear text secret will be:
~~~
        0x91, 0xa9, 0xe4, 0x22, 0x2c, 0xcb, 0xb9, 0xa9,
        0x8f, 0x14, 0xc8, 0xe1, 0xbe, 0xfd, 0x6a, 0x79,
        0xf0, 0x4e, 0x42, 0xa2, 0x4f, 0xbe, 0xb4, 0x83,
        0x1f, 0x50, 0x26, 0x80, 0x7a, 0xe8, 0x4c, 0xc3
~~~
This will produce the following key and IV:
~~~
Server AEAD key (16 bytes):
        0xc8, 0xea, 0x1b, 0xc1, 0x71, 0xe5, 0x2b, 0xae,
        0x71, 0xfb, 0x78, 0x39, 0x52, 0xc7, 0xb8, 0xfc

Server IV (12 bytes):
        0x57, 0x82, 0x3b, 0x85, 0x2c, 0x7e, 0xf9, 0xe3,
        0x80, 0x2b, 0x69, 0x0b
~~~