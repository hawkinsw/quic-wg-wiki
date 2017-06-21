The following is a straw-man list of features in the second QUIC implementation draft.

# Must Include

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft).

# Should Include

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Transport Parameter Exchange. At the very least, the four parameters specified as MUST in the draft.

* A simple single-streamed application (or, one stream in each direction). This application would ideally leverage a very simple socket API and follow simple logic easily implementable on both client on server. Brian proposed "echo and amplify" and volunteered to write it up.

* Exercise the entire life cycle of a stream, including appropriate use/handling of RST_STREAM and GOAWAY.

* Connection-level flow control (MAX_DATA, MAX_STREAM_ID frames)

* Stream level flow control (MAX_STREAM_DATA)

* Public Reset (generate where appropriate; validate & process correctly)

# Could include

* An HTTP/2 application to require multiple streams

* Address validation and HelloRetryRequest

* 0-RTT and Resumption

# Should Not Include

* Loss Recovery includes a number of concepts that are extensively tested in TCP and has low interoperability concerns. We will maintain fixed RTO-based retransmissions (i.e., no RTT calculation)

* Congestion Control

* Key Updates

* Connection ID Changes (Migration)

* PMTU Discovery