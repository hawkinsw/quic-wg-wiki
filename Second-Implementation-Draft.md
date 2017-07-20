The following is a straw-man list of features in the second QUIC implementation draft.

# Must Include

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft). These are not yet known.

* Exercise the entire life cycle of a stream, including appropriate use/handling of RST_STREAM and GOAWAY.

* Connection-level flow control (MAX_DATA, MAX_STREAM_ID frames)

* Stream level flow control (MAX_STREAM_DATA)

# Strategy 1 (Recommended?)

Lock down the unencrypted wire image to avoid middleboxes ossifying on Google-QUIC.

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Transport Parameter Exchange. At the very least, the four parameters specified as MUST in the draft.

* A simple single-streamed application (or, one stream in each direction). This application would ideally leverage a very simple socket API and follow simple logic easily implementable on both client and server. Brian proposed "echo and amplify" and volunteered to write it up.

* Address validation and HelloRetryRequest

* Stateless Reset (generate where appropriate; validate & process correctly)

Any implementations deployed at scale must also do:

* Loss Recovery beyond the exising 1-RTO retransmissions. (I believe this includes a number of concepts that are extensively tested in TCP and has low interoperability concerns).

* Congestion Control

# Strategy 2

Put in all the features that allow performance testing.

* An HTTP/2 application to require multiple streams

* 0-RTT and Resumption

* Loss Recovery beyond the exising 1-RTO retransmissions. (I believe this includes a number of concepts that are extensively tested in TCP and has low interoperability concerns). 

* Congestion Control

# Strategy 3

The objective of this plan is to build something that, while not having the full feature set, can be deployed at scale.

* Transport Parameter Exchange. At the very least, the four parameters specified as MUST in the draft.

* Address validation and HelloRetryRequest

* Stateless Reset (generate where appropriate; validate & process correctly)

* An HTTP/2 application to require multiple streams (with stateless HPACK compression, no QPACK, QCRAM, etc) and no server push.

Any implementations deployed at scale must also do:

* Loss Recovery beyond the exising 1-RTO retransmissions. (I believe this includes a number of concepts that are extensively tested in TCP and has low interoperability concerns).

* Congestion Control

# Should Not Include

* Key Updates

* Post handshake Connection ID Changes (Migration)

* PMTU Discovery