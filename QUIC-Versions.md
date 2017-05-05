Register the use of a QUIC version here so that we don't have any embarrassing collisions.

Note: These values are all defined in big endian.  Google QUIC uses little endian numbers, which can be confusing.

# Standard Space

The (WIP) QUIC specification reserves 0x00000001 to 0x0000ffff for standardized versions of the protocol.  Don't use these.  There is plenty of space available on a first-come, first-served basis.

# FCFS Space

| Version | Owner | Notes |
|---------|-------|-------|
| 0x00000000 | n/a | This value is reserved as invalid |
| 0x?a?a?a?a | IETF | Values meeting this pattern ((x&0x0f0f0f0f)==0x0a0a0a0a) are reserved for ensuring that version negotiation remains viable.  Endpoints SHOULD use these values.  Endpoints can expect that these versions will not be accepted by their peer. |
| 0x51303334 | Google | Google QUIC 34 (Q034) |
| 0x51303335 | Google | Google QUIC 35 (Q035) |
| 0x51303336 | Google | Google QUIC 36 (Q036) |
| 0x51303337 | Google | Google QUIC 37 (Q037) |
| 0x51303338 | Google | Google QUIC 38 (Q038) |
| 0x51303339 | Google | Google QUIC 39 (Q039) |
| 0x51303430 | Google | Google QUIC 40 (Q040) |
| 0xff000001 | IETF | draft-ietf-quic-transport-01 |
| 0xff000002 | IETF | draft-ietf-quic-transport-02 (see the pattern here?) |