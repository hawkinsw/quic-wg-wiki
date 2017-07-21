This document describes the next set of features to implement after we have reached the point of diminishing returns on th e [First Implementation](https://github.com/quicwg/base-drafts/wiki/First-Implementation).

The first implementation focused on the basic handshake. The second will add additional handshake features and support a rudimentary application to exercise the stream infrastructure

# New Features in this Implementation

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft). None have been identified to date.

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Exercise the entire life cycle of a stream, including appropriate use/handling of RST_STREAM.

* Connection-level flow control (MAX_DATA, MAX_STREAM_ID frames). The intent, at this point, is to provide reasonable limits to traffic burstiness and expect peers to respect them. A non-goal is to fine-tune the algorithms that generate actual flow control limits.

* Stream level flow control (MAX_STREAM_DATA). See note above.

* Transport Parameter Exchange. At the very least, the four parameters specified as MUST in the draft.

* A simple multi-streamed application. This application would ideally leverage a very simple socket API and follow simple logic easily implementable on both client and server. For instance, the client could send data on one stream and the server could echo copies of that on multiple streams. Brian proposed "echo and amplify" and volunteered to write it up. 

* Address validation and HelloRetryRequest

* Public/Stateless Reset (generate where appropriate; validate & process correctly)

# Things not in this implementation

* PMTU Discovery

* 0-RTT and Resumption

* Key Updates

* Post handshake Connection ID Changes (Migration)

* Loss Recovery beyond the exising 1-RTO retransmissions. (This involves a number of concepts that are extensively tested in TCP and has low interoperability concerns).

* Congestion Control

* HTTP/2 mapping, compression, etc.