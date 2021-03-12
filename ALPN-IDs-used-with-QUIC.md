An informal table of TLS Application-Layer Protocol Negotiation (ALPN) Protocol IDs that are known to have been used with QUIC.

While IANA holds the formal registration table of Protocol IDs at https://www.iana.org/assignments/tls-extensiontype-values/tls-extensiontype-values.xhtml#alpn-protocol-ids, there are several values that are known to be used with QUIC in the wild.



| Identification Sequence| Notes |
|---------|-------|
|h3-NN | HTTP/3 over QUIC I-D draft |
|h3 | HTTP/3 over QUIC v1 |
|h3-T0NN | Google variant of HTTP/3 over QUIC |
|h3-Q0NN | Google variant of HTTP/3 over QUIC |
|http/2+quic | Google variant of HTTP/3 over QUIC |
|http/2+quic/NN | Google variant of HTTP/3 over QUIC |
|quic | Google variant of HTTP/3 over QUIC |
|h3-fb-NN | Facebook variant of HTTP/3 over QUIC I-D draft |
|hq-NN | HTTP/0.9 over QUIC I-D draft, used for interoperability testing |
|hq | HTTP/0.9 over QUIC v1, used for interoperability testing |
|hq-interop | HTTP/0.9 over QUIC v1, used for interoperability testing |
|smb| SMB over QUIC |
|perf | Performance testing protocol over QUIC (https://datatracker.ietf.org/doc/draft-banks-quic-performance/) |
|siduck| Simple DATAGRAM test over QUIC (https://datatracker.ietf.org/doc/draft-pardue-quic-siduck/) |
|siduck-NN | Simple DATAGRAM test over QUIC |
|wq-vvv-NN | WebTransport QuicTransport (https://tools.ietf.org/html/draft-vvv-webtransport-quic-02#section-9.1) |
|doq-iNN | DNS over QUIC (https://datatracker.ietf.org/doc/draft-ietf-dprive-dnsoquic/) |
|qrt-NN | QRT: QUIC RTP Tunnelling (https://datatracker.ietf.org/doc/draft-hurst-quic-rtp-tunnelling/) |