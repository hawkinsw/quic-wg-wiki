Register the use of a QUIC version here to avoid collisions. Note: These values are all defined in big endian.

# Standard Space

The (WIP) QUIC specification reserves 0x00000001 to 0x0000ffff for standardized versions of the protocol. Don't use these for experimental versions. There is plenty of space available on a first-come, first-served basis.

# FCFS Space

| Version | Owner | Notes |
|---------|-------|-------|
| 0x00000000 | n/a | This value is reserved as invalid |
| 0x00000001 | IETF | rfc9000 |
| 0x?a?a?a?a | IETF | Values meeting this pattern ((x&0x0f0f0f0f)==0x0a0a0a0a) are reserved for ensuring that version negotiation remains viable.  Endpoints SHOULD use these values.  Endpoints can expect that these versions will not be accepted by their peer. |
| 0x454747[00-ff]| NetApp | quant | 
| 0x50435130     | Private Octopus | Picoquic internal test version | 
| 0x5130303[1-9] | Google | Google QUIC 01 - 09 (Q001 - Q009) |
| 0x5130313[0-9] | Google | Google QUIC 10 - 19 (Q010 - Q019) |
| 0x5130323[0-9] | Google | Google QUIC 20 - 29 (Q020 - Q029) |
| 0x5130333[0-9] | Google | Google QUIC 30 - 39 (Q030 - Q039) |
| 0x5130343[0-9] | Google | Google QUIC 40 - 49 (Q040 - Q049) |
| 0x5130353[0-9] | Google | Google QUIC 50 - 59 (Q050 - Q059) |
| 0x51303939 | Google | Google QUIC 99 (Q099) |
| 0x5430343[8-9] | Google | Google QUIC with TLS 48 - 49 (T048 - T049) |
| 0x5430353[0-9] | Google | Google QUIC with TLS 50 - 59 (T050 - T059) |
| 0x54303939 | Google | Google QUIC with TLS 99 (T099) |
| 0x50524f58 | Google | Proxied QUIC (PROX) |
| 0x5c10000[0-f] | Anapaya | QUIC over SCION |
| 0x51474f[0-255] | quic-go | "QGO" + [0-255]
| 0x91c170[0-255] | quicly | "qicly0" + [0-255] |
| 0xabcd000[0-f] | Microsoft | MsQuic |
| 0xf123f0c[0-f] | Mozilla | MozQuic |
| 0xfaceb00[0-f] | Facebook | mvfst |
| 0xff[000000-ffffff] | IETF | draft-ietf-quic-transport-xx |
| 0xf0f0f0f[0-f] | ETH Zürich | Measurability experiments |
| 0xf0f0f1f[0-f] | Telecom Italia | Measurability experiments |
| 0xf0f0f2f[0-f] | quinn-noise | https://github.com/ipfs-rust/quinn-noise |
| 0x0700700[0-f] | Tencent | TencentQuic |