This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools).

Please add your implementation below. Keep sorted alphabetically. There are two sections, one for "IETF QUIC" and one for "Google QUIC".

## IETF QUIC

The following stacks implement the IETF versions of QUIC.

### AppleQUIC

AppleQUIC is a test client and server implementation.  

- **Language:** C, Objective-C
- **Version:** draft-05
- **Roles:** Client, Server
- **Handshake:** TLS 1.3-21
- **Protocol IDs:** `0xff000005`
- **Public server:**

### [ats](https://cwiki.apache.org/confluence/display/TS/QUIC)

QUIC implementation in Apache Traffic Server

- **Language:** C++
- **Version:** draft-08
- **Roles:** Server
- **Handshake:** TLS 1.3-22
- **Protocol IDs:** `0xff000008`
- **Public server:** quic.ogre.com:4433


### [minq](https://www.github.com/ekr/minq)

Minimal QUIC implementation with emphasis on readability and simplicity. Very un-baked but will track the emerging document. Library plus test programs.

- **Language:** Go
- **Version:** draft-05
- **Roles:** client and server
- **Handshake:** TLS 1.3-21 or -2
- **Protocol IDs:** `0xff000004`
- **Public server:** minq.dev.mozaws.net:4433, logs available at https://minq.dev.mozaws.net:3000/<client-connection-id-hex>


### [mozquic](https://github.com/mcmanus/mozquic)
an iQUIC library meant to track standardization milestones.

- **Language:** c++ with C interface
- **Version:**  -06
- **Roles:** library, client, server
- **Handshake:**   TLS 1.3-21 or -2
- **Protocol IDs:** `0xff000005` and `0xf123f0c5` and `0xff000006` hq-05
- **Public server:** mozquic.ducksong.com 4433 and 4434 (4434 does server stateless retry validations). more info at https://github.com/mcmanus/mozquic/wiki/Endpoint-mozquic.ducksong.com-port-4433


### [ngtcp2](https://github.com/ngtcp2/ngtcp2)
ngtcp2 project is an effort to implement IETF QUIC protocol

- **Language:** C
- **Version:** draft-08
- **Roles:** client, library, server
- **Handshake:** TLSv1.3-22
- **Protocol IDs:** `0xff000008`
- **Public server:** nghttp2.org:4433


### ngx_quic

Implementation of QUIC for NGINX

- **Language:** C
- **Version:** draft-07 (but incomplete)
- **Roles:** server
- **Handshake:** TLSv1.3-21
- **Protocol IDs:** `0xff000007`
- **Public server:** TBD


### [picoquic](https://github.com/private-octopus/picoquic)
A small implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3

- **Language:** C
- **Version:** draft-05
- **Roles:** library and test tools
- **Handshake:** TLS 1.3 draft 21
- **Protocol IDs:** `0xff000005`
- **Public server:**


### [quant](https://github.com/NTAP/quant)

QUANT (QUIC Userspace Accelerated Network Transfers), the beginnings of a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework

- **Language:** C11
- **Version:** draft-08
- **Roles:** client, library, server
- **Handshake:** TLS1.3-22
- **Protocol IDs:** `0xff000008`
- **Public server:** quant.eggert.org:4433 (Logs for last run at https://quant.eggert.org/log; all logs at https://quant.eggert.org/)


### [quicly](https://github.com/h2o/quicly)

QUIC protocol implementation for H2O server

- **Language:** C
- **Version:** draft-04
- **Roles:** client and server
- **Handshake:** TLS 1.3-18
- **Protocol IDs:**
- **Public server:** kazuhooku.com:4433


### Winquic

Winquic is an implementation of QUIC on Windows.

- **Language:** C
- **Version:** draft-07
- **Roles:** client, server
- **Handshake:** TLS 1.3-21
- **Protocol IDs:** `0xff000007`
- **Public server:** msquic.westus.cloudapp.azure.com:4433

### MVFST

MVFST is the facebook implementation of QUIC

- **Language:** C++
- **Version:** draft-05
- **Roles:** client, server
- **Handshake:** TLS 1.3-21
- **Protocol IDs:** `0xff000005` 
- **Public server:** fb.mvfst.net:4433


## Google QUIC

The following stacks implement the earlier Google versions of QUIC.


### [goquic](https://github.com/devsisters/goquic)

Work-in-progress QUIC implementation for Go. This is based on libquic library.

- **Language:** Go
- **Version:** Q030, Q031, Q032, Q033, Q033, Q034, Q035, Q036.
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**


### [libquic](https://github.com/devsisters/libquic/)

Library which is made from sources and dependencies extracted from Chromium's QUIC Implementation with a few modifications and patches to minimize dependencies needed to build QUIC library.

- **Language:**  C, C++
- **Version:** Q030, Q031, Q032, Q033, Q033, Q034, Q035, Q036.
- **Roles:** library
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**



### [lsquic](https://www.litespeedtech.com/products)

LiteSpeed QUIC implementation for use with LiteSpeed server products.

- **Language:**  C, C++
- **Version:** Q035, Q037, Q038, Q039, Q041.
- **Roles:** Server and Client.  The latter is available as an [open-source library](https://github.com/litespeedtech/lsquic-client).
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**


### [proto-quic](https://github.com/google/proto-quic)

`proto-quic` is intended as a standalone library for QUIC. It contains the subset of Chromium code and dependencies require for QUIC so folks can use the Chromium code without depending on all of Chromium. It is intended to be a cross-platform library, but will only support the set (or a strict subset) of platforms which Chromium already supports.

- **Language:** C++
- **Version:**
- **Roles:** library
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**


### [quic-go](https://github.com/lucas-clemente/quic-go)

A QUIC implementation in Go. It has interop with Google QUIC (Chrome + GFE), and experimental support for IETF QUIC.

- **Language:** Go
- **Version:** Q039 (production), draft-07 (experimental)
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto (production), TLS 1.3-21 (experimental)
- **Protocol IDs:**
- **Public server:**


### [stellite](https://github.com/line/stellite)

Stellite project is a client library and server application that offers an easy way to develop, build, and implement client/server. It aims to provide fast and stable connectivity to mobile applications.

- **Language:** C++
- **Version:** 
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**
