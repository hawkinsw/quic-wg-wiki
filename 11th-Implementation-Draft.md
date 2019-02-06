## Overview

* QUIC Version draft-18 (vn=**ff000012**)

* HTTP 3 (ALPN=**h3-18**)

* HTTP 0.9/1.1 (ALPN=**hq-18**)

* Record interop results at [here](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit#gid=1965785440)

* List of implementations at [here](https://github.com/quicwg/base-drafts/wiki/Implementations)

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
|Migration | M | A new CID is offered to the peer, and it migrates the connection to it |
|Rebinding | B | The client moves to a different address or port, and the server migrates to it |
|Key Update | U | One endpoint can update keys and its peer responds correctly |
|H3 | 3 | An H3 transaction succeeded |

## Perf Features Tested

(TODO)