This document describes the next set of features to implement after we have reached the point of diminishing returns on the [First Implementation](https://github.com/quicwg/base-drafts/wiki/First-Implementation).

The first implementation focused on the basic handshake. The second will add additional handshake features and support a rudimentary application to exercise the stream infrastructure.

# New Features in this Implementation

* Critical shortcomings of the [First Implementation Draft](https://github.com/quicwg/base-drafts/wiki/First-Implementation). None have been identified to date.

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

## Handshake test

This test is performed by setting a connection from a client to a server, and verifying that the handshake completes.

## Version Negotiation test

In the version negotiation test, the client proposes a non supported version number such as 0xA1A2A3A4 to the server. The expectation is that the server will send back a version negotiation packet, after which client and server will complete the handshake. 

## Stream Data (encrypted)

The stream data test is preformed by establishing a connection, then requesting the page "index.html". The text succeeds if the data is accepted and some reply is received.

## Close

The Close test is performed by setting a connection, downloading the "index.html" page, and then sending a "Connection Close" request. The test succeeds if the connection is closed properly by server and clients.

## HTTP/0.9 exchange

The HTTP/0.9 exchange test is preformed by establishing a connection, then requesting the page "index.html" by sending the "Get /index.html\n" command in a STREAM DATA message, then signalling the FIN bit to close that stream in the sending direction. The text succeeds if the proper HTML content of the index.html page is received on the corresponding stream from the server.

## Server stateless retry

The server stateless retry (SRR) test requires configuring a server to respond to connection requests by an SRR message. Many implementers have done that by deploying two servers, a regular server on port 4433, and a server that automatically sends SRR. (The Server Stateless Retry option is sometimes designated by its TLS equivalent, Hello Retry request, HRR.)

The test is executed by having the client connect to the SRR port of the server, receive the SRR message, and then successfully complete the handshake.

## Stateless reset

TBD.

## Flow control

The flow control test is executed by having a client negotiate a small enough initial MAX DATA transport parameter, e.g. 16K or 64K, and then establishing a connection and requesting 4 files in parallel: logo.jpg, main.jpg, 1000001.txt and 999999.txt. The test succeeds if at least two of these files are correctly received.