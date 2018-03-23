This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools). Current [interop status](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit?usp=sharing); make sure you are looking at or editing the correct tab.

Please add your implementation below. Keep sorted alphabetically. There are two sections, one for "IETF QUIC" and one for "Google QUIC".

## IETF QUIC

The following stacks implement the IETF versions of QUIC.

### AppleQUIC

AppleQUIC is a test client and server implementation.  

- **Language:** C, Objective-C
- **Version:** draft-08
- **Roles:** Client, Server
- **Handshake:** TLS 1.3-22
- **Protocol IDs:** `0xff000008`
- **Public server:** 

### [ats](https://cwiki.apache.org/confluence/display/TS/QUIC)

QUIC implementation in Apache Traffic Server

- **Language:** C++
- **Version:** draft-08
- **Roles:** Server, Client
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


### mvfst

mvfst (pronounced move fast) is an implementation of QUIC by Facebook

- **Language:** C++
- **Version:** draft-09
- **Roles:** client, server, library
- **Handshake:** TLS 1.3-23
- **Protocol IDs:** `0xff000009` 
- **Public server:** fb.mvfst.net:4433


### [ngtcp2](https://github.com/ngtcp2/ngtcp2)
ngtcp2 project is an effort to implement IETF QUIC protocol

- **Language:** C
- **Version:** draft-09
- **Roles:** client, library, server
- **Handshake:** TLSv1.3-23
- **Protocol IDs:** `0xff000009`
- **Public server:** nghttp2.org:4433


### ngx_quic

Implementation of QUIC for NGINX

- **Language:** C
- **Version:** draft-09
- **Roles:** server
- **Handshake:** TLSv1.3-23
- **Protocol IDs:** `0xff000009`
- **Public server:** quic.tech:4433

### Pandora

Client and server library for our experimenting with different QUIC applications and for measurements, developed by Aalto Univ and now TUM.  Developed on GitHub but not yet public.  Server setup still a bit shake; see also Pandora in the QUIC Slack channel

- **Language:** C
- **Version:** draft-08 (mostly)
- **Roles:** library with minimal client, server
- **Handshake:** TLS -22 (using picotls)
- **Protocol IDs:** `0xff000008`
- **Public server:** pandora.cm.in.tum.de:4433 (the server log is accessible via http[s]://pandora.cm.in.tum.de/)

### [picoquic](https://github.com/private-octopus/picoquic)
A small implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3

- **Language:** C
- **Version:** draft-08
- **Roles:** library and test tools, test client, test server
- **Handshake:** TLS 1.3 draft 22
- **Protocol IDs:** `0xff000008`
- **Public server:** test.privateoctopus.com:4433


### [quant](https://github.com/NTAP/quant)

QUANT (QUIC Userspace Accelerated Network Transfers), a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework. (Also works over the standard Sockets API.)

- **Language:** C11
- **Version:** draft-09
- **Roles:** client, library, server
- **Handshake:** TLS1.3-23
- **Protocol IDs:** `0xff000009`
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
- **Version:** draft-09
- **Roles:** client, server
- **Handshake:** TLS 1.3-23
- **Protocol IDs:** `0xff000009`
- **Public server:** msquic.westus.cloudapp.azure.com:4433


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
- **Version:** Q039 (production), draft-10 (experimental)
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto (production), TLS 1.3-22 (experimental)
- **Protocol IDs:**
- **Public server:** seemann.io:443 (Q039)


### [stellite](https://github.com/line/stellite)

Stellite project is a client library and server application that offers an easy way to develop, build, and implement client/server. It aims to provide fast and stable connectivity to mobile applications.

- **Language:** C++
- **Version:**
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**

### [caddyserver](https://github.com/lucas-clemente/quic-go)

Caddy 0.9 has experimental QUIC support, powered by lucas-clemente/quic-go. Read more about [caddyservers QUIC](https://github.com/mholt/caddy/wiki/QUIC) implementation and usage

- **Language:** Go
- **Version:** since 0.9 (experimental)
- **Roles:** server
- **Handshake:** QUIC Crypto (production)
- **Protocol IDs:**
- **Public server:**
