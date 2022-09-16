This wiki tracks known implementations of QUIC. See also our [Tools listing](Tools). Current [interop status](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit?usp=sharing); make sure you are looking at or editing the correct tab.

Please add your implementation below. Keep sorted alphabetically. There are three sections, one for "IETF QUIC Transport", one for "HTTP/3C", and one for "QPACK". Entries may appear in multiple sections e.g. where a stack provides both IETF QUIC Transport and HTTP/3.

## Note ##

If you are working on a QUIC implementation, please consider joining the [QUIC Developers Slack Channel](https://quicdev.slack.com/). Also, if possible, please set up a public server and publish its details below, so others can try and interoperate with your code.

## IETF QUIC Transport

The following stacks implement the IETF versions of QUIC Transport. They may include an application layer mapping other than IETF HTTP over QUIC e.g. HTTP/0.9

### [aioquic](https://github.com/aiortc/aioquic)

QUIC implementation using Python and asyncio.

- **Language:** Python
- **Version:** draft-29 through version 1
- **Roles:** client, server, library
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001d`, `0xff00001e`, `0xff00001f`, `0xff000020`, `0x1`
- **Public server:**
  - quic.aiortc.org:443
  - quic.aiortc.org:4434 (Stateless Retry)
applicable.

### Akamai QUIC

- **Roles:** Server
- **Public server:** ietf.akaquic.com:443
- **Version:** draft-29
- **Protocol IDs:** `0xff00001d` 
- **Handshake:** TLS 1.3 RFC

### AppleQUIC

AppleQUIC is a client and server implementation.  

- **Language:** C, Objective-C, Swift
- **Version:** draft-29 and version 1
- **Roles:** Client, Server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:** `0xff00001d` + `0x00000001`
- **Public server:** N/A

### [ats](https://cwiki.apache.org/confluence/display/TS/QUIC)

QUIC implementation in Apache Traffic Server

- **Language:** C++
- **Version:** draft-29
- **Roles:** Server, Client
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001d`
- **Public server:** 
  - quic.ogre.com:4433
  - quic.ogre.com:4434 (Stateless Retry)
  - quic.ogre.com:4443 (Preferred Address 4443 -> 4433)
  - https://quic.ogre.com/cgi-bin/quic_log.cgi (Debug logs)

### [Chromium](https://quiche.googlesource.com/quiche/)	

Chromium's QUIC Implementation (draft-29 supported in Chrome 85.0.4171.0 and later).	

- **Languages:**  C, C++
- **Versions:** Q043, Q046, Q050, T050, T051, draft-27, draft-29.
- **Roles:** library, client, server
- **Handshakes:** QUIC Crypto, TLS
- **Protocol IDs:** `Q043`, `Q046`, `Q050`, `T050`, `T051`, `0xff00001b`, `0xff00001d`
- **ALPNs:** `h3-Q043`, `h3-Q046`, `h3-Q050`, `h3-T050`, `h3-T051`, `h3-27`, `h3-29`
- **Public Servers:**
  - https://quic.rocks:4433
  - https://google.com
- **Entry in Interop Matrix:** ~gQUIC

**NOTE**: Not to be confused with Cloudflare quiche.

### f5

QUIC implementation in F5 TMOS

- **Language:** C
- **Version:** draft-29 and 32
- **Roles:** Server, Client
- **Handshake:** RFC 8446
- **Protocol IDs:** `0xff00001d`, `0xff000020`
- **ALPN:** `h3-29`, `h3-32`. `hq-29` and `hq-32` available upon request.
- **Public server:** f5quic.com:4433. spin, logging, and retry on f5quic.com:4434.

### [haproxy](https://github.com/haproxy/haproxy/)
QUIC - HTTP/3 implementation in haproxy

- **Language:** C
- **Version:** draft-29, v1, v2
- **Roles:** server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0xff00001d`, `0x00000001`, `0x709a50c4`
- **ALPN:** `h3-29`, `h3`, `hq-interop`
- **Public server:** https://www.haproxy.org/, 
  look out for "Site served using" section after a reload

### [Haskell quic](https://github.com/kazu-yamamoto/quic)

- **Language:** Haskell
- **Version:** draft-29
- **Roles:** client, server, library
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001d`
- **Public server:** mew.org:443, mew.org:4434 for stateless retry

### [kwik](https://bitbucket.org/pjtr/kwik/)

Kwik is a QUIC client (and client library) implementation in Java.

- **Language:** Java
- **Version:** draft-29 through version 1
- **Roles:** client, client library, server
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001d`, `0xff00001e`, `0xff00001f`, `0xff000020`, `0x1`


### [lsquic](https://github.com/litespeedtech/lsquic)

LiteSpeed QUIC and HTTP/3 library.  Works on Linux, FreeBSD, MacOS, Android, and Windows.  Turn-key open-source web server that uses lsquic is available at [openlitespeed.org](https://openlitespeed.org/) in both source and package forms.  Bindings are available for [Crystal](https://github.com/iv-org/lsquic.cr) and [Lisp](https://github.com/AeroNotix/cl-lsquic).

- **Language:**  C
- **Version:** v1, Draft-34, Draft-29, Draft-27, Q043, Q046, and Q050.
- **Roles:** Client, Server, Library
- **Handshake:** QUIC Crypto, RFC 8446
- **Protocol IDs:** `0x00000001`, `0xFF000022`, `0xFF00001D`, `0xFF00001B`
- **Public server:**
  - http3-test.litespeedtech.com:4433, http3-test.litespeedtech.com:4434 (sends stateless retry packets), and http3-test.litespeedtech.com:4435 (faster downloads due to less logging), and :4437 (siduck-00 :duck:) for v1, ID-34, ID-29, and ID-27 as well as Google QUIC versions Q043, Q046, and Q050
    - This server supports HTTP/3 and QPACK and provides some services to test transfer of data each way.  `GET /` for details.
    - Delayed ACKs are supported -- versions -01 and -02.
  - www.litespeedtech.com:443 is our production web server.

### [MsQuic](https://github.com/microsoft/msquic)

Microsoft's general purpose (cross-platform) QUIC implementation. Optimized for [high performance](https://microsoft.github.io/msquic/). More documentation [here](https://github.com/microsoft/msquic#documentation).

- **Platforms:** Windows, Linux, macOS (alpha)
- **Language:** C
- **Version:** Draft-29 & v1
- **Roles:** client, server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:** `0xff00001D`, `0x1`
- **Public server:** msquic.net or quic.westus.cloudapp.azure.com

### [mvfst](https://github.com/facebookincubator/mvfst)

mvfst (pronounced move fast) is an implementation of QUIC by Facebook

- **Language:** C++
- **Version:** draft-29
- **Roles:** client, server, library
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001b` 
- **Public server:** fb.mvfst.net:443, www.facebook.com, www.instagram.com

### [Neqo](https://github.com/mozilla/neqo)
Mozilla/Firefox QUIC and HTTP3 implementation.

- **Language:** Rust
- **Version:**  draft-27 through version 1 and 2.
- **Roles:** library, client, server (server is primarily for client testing and has incomplete or missing support for important features like anti-amplification)
- **Handshake:**   TLS 1.3
- **Protocol IDs:** `0x1`, `0x709a50c4`; h3
- **Public server:** None

### [ngtcp2](https://github.com/ngtcp2/ngtcp2)
ngtcp2 project is an effort to implement IETF QUIC protocol

- **Language:** C
- **Version:** v1, draft-29, draft-30, draft-31, and draft-32
- **Roles:** client, library, server
- **Handshake:** TLSv1.3 (RFC 8446)
- **Protocol IDs:** `0x00000001`, `0xff00001d`, `0xff00001e`, `0xff00001f`, and `0xff000020`
- **Public server:** nghttp2.org:4433, nghttp2.org:443

### [nginx](https://hg.nginx.org/nginx-quic/)
QUIC and HTTP/3 server implementation in nginx

- **Language:** C
- **Version:** draft-27 .. draft-32
- **Roles:** server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0xff00001b` .. `0xff000020`
- **Public server:** quic.nginx.org:443 (draft-29)

### [nginx-cloudflare](https://github.com/cloudflare/quiche/tree/master/extras/nginx)
Implementation of QUIC for NGINX based on quiche, by Cloudflare.

- **Language:** C
- **Version:** draft-27, draft-28, draft-29
- **Roles:** server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0xff00001b`, `0xff00001c`, `0xff00001d`
- **Public server:** cloudflare-quic.com:443 (HTTP/3 only)

<!--
### [Node.js QUIC](https://github.com/nodejs/quic)
Implementation of QUIC and HTTP/3 support in Node.js (based on ngtcp2)

- **Language:** C++ and JavaScript
- **Version:** draft-25
- **Roles:** client, server
- **Handshake:** TLSv1.3
- **Protocol IDs:** `0xff000019`
- **Public server:** -

Notes: No public server yet. Implementation is still in development and not yet merged with main Node.js repository. 
-->

### [picoquic](https://github.com/private-octopus/picoquic)
A small(ish) implementation of QUIC in C, to explore the protocol and the API, for example for DNS over QUIC. Relies on PicoTLS for TLS 1.3. MIT license. Tested on Windows, Linux, FreeBSD/IOS.

- **Language:** C
- **Version:** draft-32/31/30/29/28/27
- **Roles:** library and test tools, test client, test server
- **Handshake:** TLS 1.3 (using picotls)
- **Protocol IDs:** `0xff000020`, `0xff00001f`, `0xff00001e`, `0xff00001d`, `0xff00001c`, `0xff00001b`
- **Public server:** test.privateoctopus.com:4433 (server log accessible at https://test.privateoctopus.com/). Use port 4434 to test retries.

### [Pluginized QUIC](https://github.com/p-quic/pquic)
The PQUIC implementation, a framework that enables QUIC clients and servers to dynamically exchange protocol plugins that extend the protocol on a per-connection basis.

- **Language:** C (and eBPF)
- **Version:** draft-29
- **Roles:** library, client and server
- **Handshake:** TLS 1.3 (using picotls)
- **Protocol IDs:** `0xff00001d`
- **Public server:** test.pquic.org:443 (server logs not yet publicly available).

### [quant](https://github.com/NTAP/quant)

QUANT (QUIC Userspace Accelerated Network Transfers), a BSD-licensed C11 implementation on top of the zero-copy [warpcore](https://github.com/NTAP/warpcore) userspace UDP/IPv4 stack for the [netmap](http://info.iet.unipi.it/~luigi/netmap/) packet I/O framework. (Also works over the standard Sockets API.)

QUANT is a general transport library and does *NOT* implement H3.

- **Language:** C
- **Version:** draft-33, draft-34, v1
- **Roles:** client, library, server
- **Handshake:** TLS1.3
- **Protocol IDs:** `0xff000021`, `0xff000022`, `0x1`
- **Public server:** quant.eggert.org:4433 (and more, see [wiki](https://github.com/NTAP/quant/wiki) for description)

### [quiche](https://github.com/cloudflare/quiche)

quiche is an implementation of the QUIC transport protocol as specified by the IETF. It provides a low level API for processing QUIC packets and handling connection state, while leaving I/O (including dealing with sockets) to the application. Example client and server are also provided.

**NOTE**: Not to be confused with Google QUICHE.

- **Language:** Rust
- **Version:** v1, draft-27, draft-28, draft-29
- **Roles:** library, client, server
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0x00000001`, `0xff00001b`, `0xff00001c`, `0xff00001d`
- **Public server:** quic.tech:4433 (HTTP/0.9 + 0-RTT) / quic.tech:8443 (HTTP/3 + 0-RTT) / quic.tech:8444 (HTTP/3 + Retry)


### [quicly](https://github.com/h2o/quicly)

QUIC protocol implementation for H2O server

- **Language:** C
- **Version:** draft-27
- **Roles:** client and server
- **Handshake:** TLS 1.3 (final)
- **Protocol IDs:**
- **Public server:**
  - quic.examp1e.net:4433 (HTTP/0.9)
  - quic.examp1e.net:443 (HTTP/3)

### [Quinn](https://github.com/djc/quinn)

Rust implementation based on tokio/futures, using rustls for TLS.

- **Language:** Rust
- **Version:** draft-28
- **Roles:** library, client, server
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001c`
- **Public server:** h3.stammw.eu:443

### [quic-go](https://github.com/lucas-clemente/quic-go)

A QUIC implementation in Go.

- **Language:** Go
- **Version:** always the current draft
- **Roles:** client, library, server
- **Handshake:** TLS 1.3 RFC
- **Protocol IDs:**
- **Public server:** https://interop.seemann.io

### [s2n-quic](https://github.com/aws/s2n-quic)

- **Language:** Rust
- **Version:** v1
- **Roles:** library, client, server
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0x00000001`

### [XQUIC](https://github.com/alibaba/xquic)

XQUIC Library released by Alibaba is a cross-platform implementation of IETF QUIC and HTTP/3 protocol.

- **Platforms:** Linux, macOS, Android, iOS
- **Language:** C
- **Version:** v1, Draft-29
- **Roles:** Client, Server, Library
- **Handshake:** TLSv1.3 (RFC8446)
- **Protocol IDs:** `0x00000001`, `0xFF00001D`


## HTTP/3

The following implement HTTP/3. The "Transport library" field identifies one (or more) of the above stacks if applicable.

### Akamai QUIC

See entry in the "IETF QUIC Transport" section.

### Chromium

See entry in the "IETF QUIC Transport" section.

### [Flupke](https://bitbucket.org/pjtr/flupke)
Flupke is a HTTP3 client build on top of Kwik. 

- **Language:** Java
- **Transport library:** [Kwik](https://bitbucket.org/pjtr/kwik)
- **HTTP3 Version:** draft-29 through version 1
- **QUIC Verson:** draft-29 through version 1
- **Roles:** client, client library, server
- **Public server:** -

### [lsquic](https://github.com/litespeedtech/lsquic)

LiteSpeed QUIC and HTTP/3 library.  Works on Linux, FreeBSD, MacOS, and Windows.  Turn-key open-source web server that uses lsquic is available at [openlitespeed.org](https://openlitespeed.org/) in both source and package forms.

- **Language:**  C
- **Transport Library:** lsquic
- **Version:** v1, Draft-34, Draft-29, Draft-27.
- **Roles:** Client, Server, Library
- **Protocol IDs:** `0x00000001`, `0xFF000022`, `0xFF00001D`, `0xFF00001B`
- **Public server:** www.litespeedtech.com:443

### [nghttp3](https://github.com/ngtcp2/nghttp3)
nghttp3 is an implementation of HTTP/3 mapping over QUIC and QPACK in C.
It does not depend on any particular QUIC transport implementation.

- **Language:** C
- **Transport library:** It does not depend on any particular QUIC transport library.
- **HTTP over QUIC Version:** draft-32
- **Roles:** library
- **Public server:** nghttp2.org:4433

### [Node.js QUIC](https://github.com/nodejs/quic)
Implementation of QUIC and HTTP/3 support in Node.js (based on nghttp3)

- **Language:** C++ and JavaScript
- **Version:** draft-25
- **Roles:** client, server
- **Handshake:** TLSv1.3
- **Protocol IDs:** `0xff000019`
- **Public server:** -

Notes: No public server yet. Implementation is still in development and not yet merged with main Node.js repository. 

### [proxygen](https://github.com/facebook/proxygen)
proxygen implements HTTP/3 mapping over QUIC and QPACK in C++, with MVFST as the transport.

- **Language:** C++
- **Transport library:** MVFST
- **HTTP over QUIC Version:** draft-23
- **Roles:** library, sample server/client
- **Public server:** fb.mvfst.net:4433, facebook.com:443

### quiche

See entry in the "IETF QUIC Transport" section.

### [XQUIC](https://github.com/alibaba/xquic)

See entry in the "IETF QUIC Transport" section.


## QPACK

### [ls-qpack](https://github.com/litespeedtech/ls-qpack)

A standalone, portable library (Linux, FreeBSD, Windows, MacOS) written in vanilla C.  Bindings are available for [Go](https://github.com/mpiraux/ls-qpack-go), [Python](https://github.com/aiortc/pylsqpack), and [TypeScript](https://github.com/rmarx/quicker/tree/draft-20/src/http/http3/common/qpack).

- **Language:** C
- **Version:** [-21](https://tools.ietf.org/html/draft-ietf-quic-qpack-21)

### f5

- **Language:** C
- **Version:** [-03](https://tools.ietf.org/html/draft-ietf-quic-qpack-03)

### [nghttp3](https://github.com/ngtcp2/nghttp3)

- **Language:** C
- **Version:** [-18](https://tools.ietf.org/html/draft-ietf-quic-qpack-19)

### [quiche](https://github.com/cloudflare/quiche)

- **Language:** Rust
- **Version:** [-21](https://tools.ietf.org/html/draft-ietf-quic-qpack-21)

### [proxygen](https://github.com/facebook/proxygen)

- **Language:** C++
- **Version:** [-10](https://tools.ietf.org/html/draft-ietf-quic-qpack-10)

### Chromium

See entry in the "IETF QUIC Transport" section.

### [XQUIC](https://github.com/alibaba/xquic)
- **Language:** C
- **Version:** [-21](https://tools.ietf.org/html/draft-ietf-quic-qpack-21)
