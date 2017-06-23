**This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools).**

Please add your implementation below. 

name | description | language | version | role(s) | handshake(s) | protocol id(s) |
--- | --- | --- | --- | --- | --- | ---
[mozquic] | an iQUIC library - eventually meant to support both transport and http features. Very nascent, but meant to track standardization milestones | c++ with C interface | | library client and server | tls | 0xff000004 and 0xf123f0c5 |

[proto-quic](https://github.com/google/proto-quic) | `proto-quic` is intended as a standalone library for QUIC. It contains the subset of Chromium code and dependencies require for QUIC so folks can use the Chromium code without depending on all of Chromium. It is intended to be a cross-platform library, but will only support the set (or a strict subset) of platforms which Chromium already supports. | C++ | | library | QUIC Crypto
[quant](https://github.com/NTAP/quant) | QUANT (QUIC Userspace Accelerated Network Transfers), the beginnings of a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework | C11 | | client, library, server | TLS 1.3 | 
[quic-go](https://github.com/lucas-clemente/quic-go) | A QUIC implementation in pure Go that has interop with Google QUIC (Chrome + GFE) | Go | | client, library, server | QUIC Crypto |
[stellite](https://github.com/line/stellite) | Stellite project is a client library and server application that offers an easy way to develop, build, and implement client/server. It aims to provide fast and stable connectivity to mobile applications. | C++ | | client, library, server | QUIC Crypto |
