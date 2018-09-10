This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools). Current [interop status](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit?usp=sharing); make sure you are looking at or editing the correct tab.

Please add your implementation below. Keep sorted alphabetically. There are four sections, one for "IETF QUIC Transport", one for "IETF HTTP over QUIC", one for "QPACK", and one for "Google QUIC". Entries may appear in multiple sections e.g. where a stack provides both IETF Transport and HTTP over QUIC.

## IETF QUIC

The following stacks implement the IETF versions of QUIC Transport. They may include an application layer mapping other than IETF HTTP over QUIC e.g. HTTP/0.9

### AppleQUIC

AppleQUIC is a test client and server implementation.  

- **Language:** C, Objective-C
- **Version:** draft-12 (w/ PR #1389)
- **Roles:** Client, Server
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000c`
- **Public server:** 

### [ats](https://cwiki.apache.org/confluence/display/TS/QUIC)

QUIC implementation in Apache Traffic Server

- **Language:** C++
- **Version:** draft-13 (+PR #1498, PR #1528)
- **Roles:** Server, Client
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000d`
- **Public server:** quic.ogre.com:4433


### f5

QUIC implementation in F5 TMOS

- **Language:** C
- **Version:** draft-11
- **Roles:** Server
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000b`
- **Public server:** 208.85.208.226:4433


### [lsquic](https://github.com/litespeedtech/lsquic-client/tree/201808291108-ietf-ID-12)

LiteSpeed QUIC client library.

- **Language:**  C
- **Version:** draft-12 plus PNE, Q035, Q039, Q043, and Q044.
- **Roles:** Client
- **Handshake:** QUIC Crypto, TLS 1.3-28
- **Protocol IDs:** `0xff00000c`
- **Public server:** 159.203.94.96:1234


### [minq](https://www.github.com/ekr/minq)

Minimal QUIC implementation with emphasis on readability and simplicity. Very un-baked but will track the emerging document. Library plus test programs.

- **Language:** Go
- **Version:** draft-11 (almost)
- **Roles:** client and server
- **Handshake:** TLS 1.3-28 (with https://github.com/bifurcation/mint/pull/196)
- **Protocol IDs:** `0xff00000b`
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
- **Version:** draft-13
- **Roles:** client, library, server
- **Handshake:** TLSv1.3-28
- **Protocol IDs:** `0xff00000d`
- **Public server:** nghttp2.org:4433


### ngx_quic
Implementation of QUIC for NGINX by Cloudflare

- **Language:** C
- **Version:** draft-13
- **Roles:** server
- **Handshake:** TLSv1.3-28
- **Protocol IDs:** `0xff00000d`
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
A small(ish) implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3. MIT license. Tested on Windows, Linux, FreeBSD.

- **Language:** C
- **Version:** draft-11 (draft-09 and draft-10 available in separate branches on Github)
- **Roles:** library and test tools, test client, test server
- **Handshake:** TLS 1.3 draft 28 (also supports 26 and 27)
- **Protocol IDs:** `0xff00000b` (`0xff000009` if compiling the draft-09 branch, or `0xff00000a in the draft-10 branch)
- **Public server:** test.privateoctopus.com:4433


### [quant](https://github.com/NTAP/quant)

QUANT (QUIC Userspace Accelerated Network Transfers), a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework. (Also works over the standard Sockets API.)

- **Language:** C11
- **Version:** draft-12 + [PR1389](https://github.com/quicwg/base-drafts/pull/1389)
- **Roles:** client, library, server
- **Handshake:** TLS1.3-28
- **Protocol IDs:** `0xff00000c`
- **Public server:** quant.eggert.org:4433 (see [wiki](https://github.com/NTAP/quant/wiki) for description)


### [QUICker](https://github.com/rmarx/quicker)

QUICker is a NodeJS/TypeScript based QUIC implementation from the University of Hasselt, Belgium. NodeJS C++ changes and TLS1.3 integration at https://github.com/kevin-kp/node/tree/add_quicker_support

- **Language:** TypeScript
- **Version:** draft-11
- **Roles:** client,server, library
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000b`
- **Public server:** quicker.edm.uhasselt.be:4433


### [quicly](https://github.com/h2o/quicly)

QUIC protocol implementation for H2O server

- **Language:** C
- **Version:** draft-09
- **Roles:** client and server
- **Handshake:** TLS 1.3-23
- **Protocol IDs:**
- **Public server:** kazuhooku.com:4433


### [quicr](https://github.com/Ralith/quicr)

Rust implementation with a pure state machine and futures-based async I/O. TLS provided by OpenSSL.

- **Language:** Rust
- **Version:** draft-11
- **Roles:** library, client, server
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000b`
- **Public server:** ralith.com:4433

### [Quinn](https://github.com/djc/quinn)

Rust implementation based on tokio/futures, using rustls for TLS.

- **Language:** Rust
- **Version:** draft-11
- **Roles:** library, client, server
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000b`
- **Public server:** xavamedia.nl:4433

### Winquic

Winquic is an implementation of QUIC on Windows.

- **Language:** C
- **Version:** draft-13 (+PR #1498, PR #1528)
- **Roles:** client, server
- **Handshake:** TLS 1.3-28
- **Protocol IDs:** `0xff00000d`
- **Public server:** msquic.westus.cloudapp.azure.com:4433

## IETF HTTP over QUIC

The following implement IETF HTTP over QUIC. The "Transport library" field identifies one (or more) of the above stacks if applicable.

### [nghq](https://github.com/bbc/nghq)
nghq implements the HTTP over QUIC mapping atop ngtcp2.

- **Language:** C
- **Transport library:** ngtcp2
- **HTTP over QUIC Version:** draft-09
- **Roles:** library, (non-IETF roles: multicast receiver, multicast sender)
- **Handshake:** TBD
- **Public server:** TBD

## QPACK

### [ls-qpack](https://github.com/litespeedtech/ls-qpack)

- **Language:** C
- **Version:** [-02](https://tools.ietf.org/html/draft-ietf-quic-qpack-02)

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
- **Version:** Q035, Q039, Q043, and Q044.
- **Roles:** Server and Client.  The latter is available as an [open-source library](https://github.com/litespeedtech/lsquic-client).
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
- **Public server:** seemann.io:443 (Q039), [henrock.net:443](https://henrock.net) (Q039, Q042 & Q043)


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
- **Public server:** [henrock.net:443](https://henrock.net) (Q039, Q042 & Q043)