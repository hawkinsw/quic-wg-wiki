The following is a straw-man list of features in the second QUIC implementation draft.

# Must Include

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft).

# Should Include

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Transport Parameter Exchange

* A simple single-streamed application (or, one stream in each direction) - exercise the entire life cycle of a stream.

* Connection-level flow control (MAX_DATA frame)

* Public Reset

# Could include

* An HTTP/2 application to require multiple streams

* Stream level flow control

* Address validation and HelloRetryRequest

* 0-RTT and Resumption

# Should Not Include

* Loss Recovery includes a number of concepts that are extensively tested in TCP and has low interoperability concerns. We will maintain fixed RTO-based retransmissions (i.e., no RTT calculation)

* Congestion Control

* Key Updates

* Connection ID Changes (Migration)

* PMTU Discovery