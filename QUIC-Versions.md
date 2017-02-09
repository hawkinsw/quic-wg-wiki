Register the use of a QUIC version here so that we don't have any embarrassing collisions.

Note: These values are all defined in big endian.  Google QUIC uses little endian numbers, which can be confusing.

# Standard Space

The (WIP) QUIC specification reserves 0x00000001 to 0x0000ffff for standardized versions of the protocol.  Don't use these.  There is plenty of space available on a first-come, first-served basis.

# FCFS Space

| Version | Owner | Notes |
|---------|-------|-------|
| 0x00000000 | n/a | This value is reserved as invalid |
| 0x?a?a?a?a | IETF | Values meeting this pattern ((x&0x0f0f0f0f)==0x0a0a0a0a) are reserved for ensuring that version negotiation remains viable.  Endpoints SHOULD use these values.  Endpoints can expect that these versions will not be accepted by their peer. |
| 0x22000000 | Google | Google QUIC 34 |
| 0x23000000 | Google | Google QUIC 35 |
| 0x24000000 | Google | Google QUIC 36 |
| 0x25000000 | Google | Google QUIC 37 |
| 0x26000000 | Google | Google QUIC 38 (maybe) |
| 0xff000001 | IETF | draft-ietf-quic-transport-01 |
| 0xff000002 | IETF | draft-ietf-quic-transport-02 (see the pattern here?) |