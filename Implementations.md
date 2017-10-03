**This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools).**

Please add your implementation below. 

name | description | language | version | role(s) | handshake(s) | protocol id(s) |
--- | --- | --- | --- | --- | --- | ---
[minq](https://www.github.com/ekr/minq) | Minimal QUIC implementation with emphasis on readability and simplicity. Very un-baked but will track the emerging document. Library plus test programs. | Go | draft-04 + editor's changes | client and server | TLS 1.3-21 or -20| 0xff000004 |
[lsquic](https://www.litespeedtech.com/products)  |  LiteSpeed QUIC implementation for use with LiteSpeed server products. | C, C++ | Q035, Q037 | Server | QUIC Crypto 
[mozquic](https://github.com/mcmanus/mozquic) | an iQUIC library - eventually meant to support both transport and http features. Very nascent, but meant to track standardization milestones. | c++ with C interface | | library client and server | tls 1.3 -21; plausibly -18| 0xff000005 and 0xf123f0c5 hq-05
[ngtcp2](https://github.com/ngtcp2/ngtcp2) | ngtcp2 project is an effort to implement IETF QUIC protocol  | C | draft-05 | client, library, server | TLSv1.3-21 | 0xff000005
[picoquic](https://github.com/private-octopus/picoquic) | A small implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3 | C | draft-05 | library and test tools | TLS 1.3 draft 21 | 0xff000005
[proto-quic](https://github.com/google/proto-quic) | `proto-quic` is intended as a standalone library for QUIC. It contains the subset of Chromium code and dependencies require for QUIC so folks can use the Chromium code without depending on all of Chromium. It is intended to be a cross-platform library, but will only support the set (or a strict subset) of platforms which Chromium already supports. | C++ | | library | QUIC Crypto
[quant](https://github.com/NTAP/quant) | QUANT (QUIC Userspace Accelerated Network Transfers), the beginnings of a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework | C11 | | client, library, server | TLS 1.3 | 
[quic-go](https://github.com/lucas-clemente/quic-go) | A QUIC implementation in pure Go that has interop with Google QUIC (Chrome + GFE) | Go | | client, library, server | QUIC Crypto |
[quicly](https://github.com/h2o/quicly) | QUIC protocol implementation for H2O server | C | draft-04 | client and server | TLS 1.3-18 |
[stellite](https://github.com/line/stellite) | Stellite project is a client library and server application that offers an easy way to develop, build, and implement client/server. It aims to provide fast and stable connectivity to mobile applications. | C++ | | client, library, server | QUIC Crypto |
[Wireshark](https://code.wireshark.org/review/#/c/22366/) | Wireshark is Network Analyzer Tools  | C | draft-05 | Tools |  | 0xff000005
[ats](https://cwiki.apache.org/confluence/display/TS/QUIC) | QUIC implementation in Apache Traffic Server | C++ | draft-05 | Server | TLS 1.3-21 | 0xff000005
