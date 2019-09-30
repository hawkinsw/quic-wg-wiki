This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools). Current [interop status](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit?usp=sharing); make sure you are looking at or editing the correct tab.

Please add your implementation below. Keep sorted alphabetically. There are four sections, one for "IETF QUIC Transport", one for "IETF HTTP over QUIC", one for "QPACK", and one for "Google QUIC". Entries may appear in multiple sections e.g. where a stack provides both IETF Transport and HTTP over QUIC.

## Note ##

If you are working on a QUIC implementation, please consider joining the [QUIC Developers Slack Channel](https://quicdev.slack.com/). Also, if possible, please set up a public server and publish its details below, so others can try and interoperate with your code.

## IETF QUIC

The following stacks implement the IETF versions of QUIC Transport. They may include an application layer mapping other than IETF HTTP over QUIC e.g. HTTP/0.9

### [aioquic](https://github.com/aiortc/aioquic)

QUIC implementation using Python and asyncio.

- **Language:** Python
- **Version:** draft-23
- **Roles:** client, server, library
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff000017`, `0xff000016`
- **Public server:**
  - quic.aiortc.org:443
  - quic.aiortc.org:4434 (Stateless Retry)

### AppleQUIC

AppleQUIC is a client and server implementation.  

- **Language:** C, Objective-C
- **Version:** draft-20
- **Roles:** Client, Server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:** `0xff000014`
- **Public server:** N/A

### [ats](https://cwiki.apache.org/confluence/display/TS/QUIC)

QUIC implementation in Apache Traffic Server

- **Language:** C++
- **Version:** draft-23
- **Roles:** Server, Client
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff000017`
- **Public server:** 
  - quic.ogre.com:4433
  - quic.ogre.com:4434 (Stateless Retry)
  - quic.ogre.com:4443 (Preferred Address 4443 -> 4433)
  - https://quic.ogre.com/cgi-bin/quic_log.cgi (Debug logs)

### f5

QUIC implementation in F5 TMOS

- **Language:** C
- **Version:** draft-21
- **Roles:** Server, Client
- **Handshake:** RFC 8446
- **Protocol IDs:** `0xff000016`
- **ALPN:** `hq-22`, `h3-22`
- **Public server:** 204.134.187.194:4433

Note: This server always sends RETRY packets, but we can disable this on request.

### [kwik](https://bitbucket.org/pjtr/kwik/)

Kwik is a QUIC client (library) in Java.

- **Language:** Java
- **Version:** draft-17, draft-18, draft-19, draft-20, draft-22, draft-23
- **Roles:** client
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff000011`, `0xff000012`, `0xff000013`, `0xff000014`, `0xff000016`, `0xff000017`     


### [lsquic](https://github.com/litespeedtech/lsquic)

LiteSpeed QUIC library.  Works on Linux, FreeBSD, MacOS, and Windows.  Turn-key open-source web server that uses lsquic is available at [openlitespeed.org](https://openlitespeed.org/) in both source and package forms.

- **Language:**  C
- **Version:** Draft-23, Q039, Q043, and Q046.
- **Roles:** Client, Server, Library
- **Handshake:** QUIC Crypto, RFC 8446
- **Protocol IDs:** `0xff000017`
- **Public server:**
  - http3-test.litespeedtech.com:4433, http3-test.litespeedtech.com:4434 (sends stateless retry packets), and http3-test.litespeedtech.com:4435 (faster downloads due to less logging) for ID-22 as well as Google QUIC versions Q039, Q043, and Q046
    - This server supports HTTP/3 and QPACK and provides some services to test transfer of data each way.  `GET /` for details.
  - www.litespeedtech.com:443 for the standard fare of Google QUIC versions.

### [mozquic](https://github.com/mcmanus/mozquic)
an iQUIC library meant to track standardization milestones.

- **Language:** c++ with C interface
- **Version:**  -06
- **Roles:** library, client, server
- **Handshake:**   TLS 1.3-21 or -2
- **Protocol IDs:** `0xff000005` and `0xf123f0c5` and `0xff000006` hq-05
- **Public server:** mozquic.ducksong.com 4433 and 4434 (4434 does server stateless retry validations). more info at https://github.com/mcmanus/mozquic/wiki/Endpoint-mozquic.ducksong.com-port-4433


### msquic

Microsoft's general purpose QUIC implementation.

- **Language:** C
- **Version:** draft-23
- **Roles:** client, server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:** `0xff000017`
- **Public server:** quic.westus.cloudapp.azure.com:4433 (retry on 4434)


### [mvfst](https://github.com/facebookincubator/mvfst)

mvfst (pronounced move fast) is an implementation of QUIC by Facebook

- **Language:** C++
- **Version:** draft-22
- **Roles:** client, server, library
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff000016` 
- **Public server:** fb.mvfst.net:4433

### [ngtcp2](https://github.com/ngtcp2/ngtcp2)
ngtcp2 project is an effort to implement IETF QUIC protocol

- **Language:** C
- **Version:** draft-23
- **Roles:** client, library, server
- **Handshake:** TLSv1.3 (RFC 8446)
- **Protocol IDs:** `0xff000017`
- **Public server:** nghttp2.org:4433


### ngx_quic
Implementation of QUIC for NGINX based on quiche by Cloudflare

- **Language:** C
- **Version:** draft-23
- **Roles:** server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0xff000017`
- **Public server:** cloudflare-quic.com:443 (HTTP/3 only)

### Pandora

Client and server library for our experimenting with different QUIC applications and for measurements, developed by Aalto Univ and now TUM.  Developed on GitHub but not yet public.  Server setup still a bit shake; see also pandora in the QUIC Slack channel

- **Language:** C
- **Version:** draft-20 (mostly)
- **Roles:** library with minimal client, server
- **Handshake:** TLSv1.3 (using picotls)
- **Protocol IDs:** `0xff000014`
- **Public server:** 
  - pandora.cm.in.tum.de:4433
  - pandora.cm.in.tum.de:4434 (Stateless Retry)
  - https://pandora.cm.in.tum.de (Debug logs)

pandora.cm.in.tum.de:4433 (the server log is accessible via http[s]://pandora.cm.in.tum.de/)

### [picoquic](https://github.com/private-octopus/picoquic)
A small(ish) implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3. MIT license. Tested on Windows, Linux, FreeBSD/IOS.

- **Language:** C
- **Version:** draft-22/23
- **Roles:** library and test tools, test client, test server
- **Handshake:** TLS 1.3 (also supports 26, 27 and 28) + support for QUIC extension (suppress EOED)
- **Protocol IDs:** `0xff000016`, `0xff000017`
- **Public server:** test.privateoctopus.com:4433 (server log accessible at https://test.privateoctopus.com/)

### [quant](https://github.com/NTAP/quant)

QUANT (QUIC Userspace Accelerated Network Transfers), a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework. (Also works over the standard Sockets API.)

QUANT is a general transport library and does *NOT* implement H3.

- **Language:** C11
- **Version:** draft-22
- **Roles:** client, library, server
- **Handshake:** TLS1.3 final
- **Protocol IDs:** `0xff000016`
- **Public server:** quant.eggert.org:4433 (and more, see [wiki](https://github.com/NTAP/quant/wiki) for description)

### [quiche](https://github.com/cloudflare/quiche)

quiche is an implementation of the QUIC transport protocol as specified by the IETF. It provides a low level API for processing QUIC packets and handling connection state, while leaving I/O (including dealing with sockets) to the application. Example client and server are also provided.

- **Language:** Rust
- **Version:** draft-23
- **Roles:** library, client, server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0xff000017`
- **Public server:** quic.tech:4433 (HTTP/0.9) / quic.tech:8443 (HTTP/3)

### [QUICker](https://github.com/rmarx/quicker)

QUICker is a NodeJS/TypeScript based QUIC and HTTP/3 implementation from the University of Hasselt, Belgium. 
NodeJS C++ changes and TLS1.3 integration at https://github.com/kevin-kp/node/tree/add_quicker_support-draft-18
Live logs available at https://quic.edm.uhasselt.be/quicker/logs/

- **Language:** TypeScript
- **Version:** draft-20
- **Roles:** client,server,library
- **Handshake:** TLS 1.3 (RFC8446)
- **Protocol IDs:** `0xff000014`
- **Public server:** quicker.edm.uhasselt.be:4433 (both HTTP/0.9 and HTTP/3 based on ALPN)


### [quicly](https://github.com/h2o/quicly)

QUIC protocol implementation for H2O server

- **Language:** C
- **Version:** draft-18
- **Roles:** client and server
- **Handshake:** TLS 1.3 (final)
- **Protocol IDs:**
- **Public server:**
  - kazuhooku.com:4433 (HTTP/0.9)
  - kazuhooku.com:8443 (HTTP/3)

### [Quinn](https://github.com/djc/quinn)

Rust implementation based on tokio/futures, using rustls for TLS.

- **Language:** Rust
- **Version:** draft-23
- **Roles:** library, client, server
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff000014`
- **Public server:** ralith.com:4433

### sora_quic

QUIC protocol implementation for WebRTC SFU (Selective Forwarding Unit)

- **Language:** Erlang/OTP
- **Version:** draft-19
- **Roles:** server,library
- **Handshake:** TLS 1.3 (RFC 8446)
- **Protocol IDs:** `0xff000013`

### [quic-go](https://github.com/lucas-clemente/quic-go)

A QUIC implementation in Go.

- **Language:** Go
- **Version:** draft-22
- **Roles:** client, library, server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:**
- **Public server:** -

## IETF HTTP over QUIC

The following implement IETF HTTP over QUIC. The "Transport library" field identifies one (or more) of the above stacks if applicable.

### [Flupke](https://bitbucket.org/pjtr/flupke)
Flupke is a HTTP3 client build on top of Kwik. 

- **Language:** Java
- **Transport library:** [Kwik](https://bitbucket.org/pjtr/kwik)
- **HTTP3 Version:** draft-22
- **QUIC Verson:** draft-22
- **Roles:** client library
- **Public server:** n.a. (client only)

### [nghq](https://github.com/bbc/nghq)
nghq implements the HTTP over QUIC mapping atop ngtcp2.

- **Language:** C
- **Transport library:** ngtcp2
- **HTTP over QUIC Version:** draft-09
- **Roles:** library, (non-IETF roles: multicast receiver, multicast sender)
- **Handshake:** TBD
- **Public server:** TBD

### [nghttp3](https://github.com/ngtcp2/nghttp3)
nghttp3 is an implementation of HTTP/3 mapping over QUIC and QPACK in C.
It does not depend on any particular QUIC transport implementation.

- **Language:** C
- **Transport library:** It does not depend on any particular QUIC transport library.
- **HTTP over QUIC Version:** draft-23
- **Roles:** library
- **Public server:** nghttp2.org:4433

### [proxygen](https://github.com/facebook/proxygen)
proxygen implements HTTP/3 mapping over QUIC and QPACK in C++, with MVFST as the transport.

- **Language:** C++
- **Transport library:** MVFST
- **HTTP over QUIC Version:** draft-22
- **Roles:** library, sample server/client
- **Public server:** fb.mvfst.net:4433, facebook.com:443


## QPACK

### [ls-qpack](https://github.com/litespeedtech/ls-qpack)

A standalone, portable library (Linux, FreeBSD, Windows, MacOS) written in vanilla C.  Bindings are available for [Go](https://github.com/mpiraux/ls-qpack-go), [Python](https://github.com/aiortc/pylsqpack), and [TypeScript](https://github.com/rmarx/quicker/tree/draft-20/src/http/http3/common/qpack).

- **Language:** C
- **Version:** [-10](https://tools.ietf.org/html/draft-ietf-quic-qpack-10)

### f5

- **Language:** C
- **Version:** [-03](https://tools.ietf.org/html/draft-ietf-quic-qpack-03)

### [nghttp3](https://github.com/ngtcp2/nghttp3)

- **Language:** C
- **Version:** [-10](https://tools.ietf.org/html/draft-ietf-quic-qpack-10)

### [quiche](https://github.com/cloudflare/quiche)

- **Language:** Rust
- **Version:** [-06](https://tools.ietf.org/html/draft-ietf-quic-qpack-06)

### [proxygen](https://github.com/facebook/proxygen)

- **Language:** C++
- **Version:** [-10](https://tools.ietf.org/html/draft-ietf-quic-qpack-10)


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


### [Chromium](https://www.chromium.org/quic/playing-with-quic)

Chromium's QUIC Implementation.

- **Language:**  C, C++
- **Version:** Q039, Q043, Q046, draft-23.
- **Roles:** library, client, server
- **Handshake:** QUIC Crypto, TLS
- **Protocol IDs:** `0xff000017`
- **ALPN:** `h3-23`
- **Public server:** https://quic.rocks:4433/
- **Entry in Interop Matrix:** ~gQUIC


### [lsquic](https://www.litespeedtech.com/products)

LiteSpeed QUIC implementation for use with LiteSpeed server products.

- **Language:**  C, C++
- **Version:** Q039, Q043, and Q046.
- **Roles:** Server and Client.  Both are available as an [open-source library](https://github.com/litespeedtech/lsquic).  Alternatively, use the full-featured web server: [OpenLiteSpeed](https://openlitespeed.org/).
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:**


### [quic-go](https://github.com/lucas-clemente/quic-go)

A QUIC implementation in Go. It has interop with Google QUIC (Chrome + GFE), and experimental support for IETF QUIC.

- **Language:** Go
- **Version:** Q039, Q043, Q044
- **Roles:** client, library, server
- **Handshake:** QUIC Crypto
- **Protocol IDs:**
- **Public server:** seemann.io:443 (Q039), [hnrk.io:443](https://hnrk.io) (Q039, Q043 & Q044)


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
- **Public server:** [hnrk.io:443](https://hnrk.io) (Q039, Q043 & Q044)