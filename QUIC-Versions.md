Register the use of a QUIC version here to avoid collisions. Note: These values are all defined in big endian.

# Standard Space

The (WIP) QUIC specification reserves 0x00000001 to 0x0000ffff for standardized versions of the protocol. Don't use these for experimental versions. There is plenty of space available on a first-come, first-served basis.

# FCFS Space

| Version | Owner | Notes |
|---------|-------|-------|
| 0x00000000 | n/a | This value is reserved as invalid |
| 0x?a?a?a?a | IETF | Values meeting this pattern ((x&0x0f0f0f0f)==0x0a0a0a0a) are reserved for ensuring that version negotiation remains viable.  Endpoints SHOULD use these values.  Endpoints can expect that these versions will not be accepted by their peer. |
| 0x50435130     | Private Octopus | Picoquic internal test version | 
| 0x5130303[1-9] | Google | Google QUIC 01 - 09 (Q001 - Q009) |
| 0x5130313[0-9] | Google | Google QUIC 10 - 19 (Q010 - Q019) |
| 0x5130323[0-9] | Google | Google QUIC 20 - 29 (Q020 - Q029) |
| 0x5130333[0-9] | Google | Google QUIC 30 - 39 (Q030 - Q039) |
| 0x5130343[0-9] | Google | Google QUIC 40 - 49 (Q040 - Q049) |
| 0x51474f[0-255] | quic-go | "QGO" + [0-255]
| 0x91c170[0-255] | quicly | "qicly0" + [0-255] |
| 0xabcd000[0-f] | Microsoft | WinQuic |
| 0xf10000[00-ff] | IETF | QUIC-LB |
| 0xf123f0c[0-f] | Mozilla | MozQuic |
| 0xfaceb00[0-f] | Facebook | mvfst |
| 0xff000001 | IETF | draft-ietf-quic-transport-01 |
| 0xff000002 | IETF | draft-ietf-quic-transport-02 |
| 0xff000003 | IETF | draft-ietf-quic-transport-03 |
| 0xff000004 | IETF | draft-ietf-quic-transport-04 |
| 0xff000005 | IETF | draft-ietf-quic-transport-05 |
| 0xff000006 | IETF | draft-ietf-quic-transport-06 |
| 0xff000007 | IETF | draft-ietf-quic-transport-07 |
| 0xff000008 | IETF | draft-ietf-quic-transport-08 |
| 0xff000009 | IETF | draft-ietf-quic-transport-09 |
| 0xff00000a | IETF | draft-ietf-quic-transport-10 |
| 0xff00000b | IETF | draft-ietf-quic-transport-11 |
| 0xf0f0f0f[0-f] | ETH Zürich | Measurability experiments |