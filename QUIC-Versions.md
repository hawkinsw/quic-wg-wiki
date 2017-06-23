Register the use of a QUIC version here to avoid collisions. Note: These values are all defined in big endian.

# Standard Space

The (WIP) QUIC specification reserves 0x00000001 to 0x0000ffff for standardized versions of the protocol. Don't use these for experimental versions. There is plenty of space available on a first-come, first-served basis.

# FCFS Space

| Version | Owner | Notes |
|---------|-------|-------|
| 0x00000000 | n/a | This value is reserved as invalid |
| 0x?a?a?a?a | IETF | Values meeting this pattern ((x&0x0f0f0f0f)==0x0a0a0a0a) are reserved for ensuring that version negotiation remains viable.  Endpoints SHOULD use these values.  Endpoints can expect that these versions will not be accepted by their peer. |
| 0x5130330[1-9] | Google | Google QUIC 01 - 09 (Q001 - Q009) |
| 0x5130331[0-9] | Google | Google QUIC 10 - 19 (Q010 - Q019) |
| 0x5130332[0-9] | Google | Google QUIC 20 - 29 (Q020 - Q029) |
| 0x5130333[0-9] | Google | Google QUIC 30 - 39 (Q030 - Q039) |
| 0x5130334[0-9] | Google | Google QUIC 40 - 49 (Q040 - Q049) |
| 0xf123f0c[0-f] | Mozilla | MozQuic |
| 0xff000001 | IETF | draft-ietf-quic-transport-01 |
| 0xff000002 | IETF | draft-ietf-quic-transport-02 |
| 0xff000003 | IETF | draft-ietf-quic-transport-03 |
| 0xff000004 | IETF | draft-ietf-quic-transport-04 |