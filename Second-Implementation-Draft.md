This document describes the next set of features to implement after we have reached the point of diminishing returns on the [First Implementation](https://github.com/quicwg/base-drafts/wiki/First-Implementation).

The first implementation focused on the basic handshake. The second will add additional handshake features and support a rudimentary application to exercise the stream infrastructure.

# New Features in this Implementation

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation-Draft). None have been identified to date.

* Further revisions to mechanisms in the First Implementation Draft (e.g. changes to the public header format, connection close).

* Transport Parameter Exchange. At the very least, the four parameters specified as MUST in the draft. (TODO: is this four, or five? Per draft-05, servers must send the stateless reset secret, and clients must understand it)

* Address validation and HelloRetryRequest

* Exercise the entire life cycle of a stream, including appropriate use/handling of RST_STREAM. Streams are *bidirectional*, regardless of ongoing issue discussion.

* Connection-level flow control (MAX_DATA, MAX_STREAM_ID frames). The intent, at this point, is to provide reasonable limits to traffic burstiness and expect peers to respect them. A non-goal is to fine-tune the algorithms that generate optimal flow control limits.

* Stream level flow control (MAX_STREAM_DATA). See note above.

* A simple multi-streamed application. This application would ideally leverage a very simple socket API and follow simple logic easily implementable on both client and server. The client will send simple HTTP/0.9 GETs (https://www.w3.org/Protocols/HTTP/AsImplemented.html) including a stream FIN and the server will respond in the response stream. Any relationship to the future HTTP/2 mapping is incidental.

* Public/Stateless Reset (generate where appropriate; validate & process correctly)

# Things not in this implementation (additional effort is always welcome)

* PMTU Discovery

* 0-RTT and Resumption

* Key Updates

* Post handshake Connection ID Changes (Migration)

* Loss Recovery beyond the exising 1-RTO retransmissions. (This involves a number of concepts that are extensively tested in TCP and has low interoperability concerns).

* Congestion Control

* HTTP/2 mapping, compression, etc.

# Testing processes and server expectations

The following test procedures allow participants to verify interoperability and test a set of
functions, such as basic connectivity, flow control, negotiation, etc. The tests are performed
by clients connecting to servers. We design these procedures with the following considerations
in mind:

* We want to support two kinds of servers, the "static server serving a small set of files", and the "test server making up pages on the fly".

* We want to support two kinds of clients, those that just perform a series of download according to a simple script,
and those that start by downloading a home page, and then load the documents referenced in that home page.

* We want to enable "flow control" tests, in which two large enough documents are loaded in parallel.

We thus suggest that all servers can serve a home page (index.html), and at least 2 of the following four documents:

* logo.jpg: a large JPEG image, displaying something like the QUIC logo or the logo of the server.

* main.jpg: a large JPEG image, displaying whatever the implementer sees fit. (Please keep it safe for work.)

* 1000001.txt: a text file containing 1000001 ASCII characters, organized as a set of line feed terminated lines.

* 999999.txt: a text file containing 999999 ASCII characters, organized as a set of line feed terminated lines.

Transfers can be verified by checking that the JPG images display correctly, or by verifying a checksum contained in the last line of text files (TBD).