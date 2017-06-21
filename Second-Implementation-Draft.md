The following is a straw-man list of features in the second QUIC implementation draft.

# Must Include

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft).

# Should Include

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Transport Parameter Exchange

* Address validation and HelloRetryRequest

* A simple single-streamed application (or, one stream in each direction)

* Connection-level flow control (MAX_DATA frame)

* Public Reset

# Could include

* An HTTP/2 application to require multiple streams

* Stream level flow control

* 0-RTT and Resumption

# Should Not Include

* Loss Recovery includes a number of concepts that are extensively tested in TCP and has low interoperability concerns. We will maintain fixed RTO-based retransmissions (i.e., no RTT calcuation)

* Congestion Control

* Key Updates

* Connection ID Changes

* PMTU Discovery