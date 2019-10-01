## Overview

* QUIC Version draft-23 (vn=**ff000017**)

* HTTP 3 (ALPN=**h3-23**)

* HTTP 0.9/1.1 (ALPN=**hq-23**)

* Record interop results [here](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit#gid=1510984897)

* List of implementations [here](https://github.com/quicwg/base-drafts/wiki/Implementations)

## Core Features Tested

|Feature | code | details  |
|--------------------|:---:|------------------------|
|Version Negotiation | V | A version negotiation response is elicited and acted on |
|Handshake | H | The handshake completes successfully |
|Stream Data | D | Stream data is being exchanged and ACK'ed |
|Connection Close | C | The connection close procedure completes w/a zero error code |
|Resumption | R | Connection is established using TLS Resume Ticket |
|0-RTT | Z | 0-RTT data is being sent and acted on |
|Stateless Retry | S | A handshake that includes a Retry packet completes successfully |

## Optional Features Tested

(Note that H3 and Key Update are not "optional" in the spec, but many implementations have chosen to defer them)

|Feature | code | details  |
|--------------------|:---:|------------------------|
|Migration | M | A new CID is offered to the peer, and it migrates the connection to it |
|Rebinding | B | The client moves to a different address or port, and the server migrates to it |
|Key Update | U | One endpoint can update keys and its peer responds correctly |
|H3 | 3 | An H3 transaction succeeded |
|Spin | P | A connection with the spin succeeds and the bit is spinning |
|ECN | E | Set ECT(0) on outgoing packets and verify that ACK frames with ECN information are received. (Note: test can fail if path bleaches ECN) |
|Logging | L | There is some form of **structured** logging available from both the implementations (e.g., qlog, quic-trace) |

## Perf Features Tested
|Feature | code | details  |
|--------------------|:---:|------------------------|
|Throughput | T | Download objects named 5000000 and 10000000 on separate connections over both HTTP/2 and HTTP/3 (0.9 if H3 not available). Overall transfer time should be no worse than 10% slower than HTTP/2. |

## H3 Features tested
|Feature | code | details  |
|--------------------|:---:|------------------------|
|Dynamic Tables | d | QPACK Dynamic Tables |
|Push | p | PUSH succesfully delivered |