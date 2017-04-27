This version of the implementation draft will concentrate on just the handshake.

Implementing all of QUIC is difficult, if not impossible, at this stage, so we're going to look more closely at the basic handshake only.  Specifically:

* version negotiation - all server implementations will handle packets with an unknown version and arbitrary payloads and respond with a version negotiation packet; client implementations will consume this and either abort or select a compatible version (the expectation is that only one version will be implemented, but clients could implement greasing)

* the TLS stream

* basic STREAM and ACK frame support - this includes packetization for STREAM frames on stream 0, transmitting those STREAM frames in unencrypted packets, receiving STREAM frames and assembling them into a stream of bytes, sending ACK frames in response (no timestamps needed, those are not permitted in cleartext anyway), receiving ACK frames, basic recovery (this might be very basic, but it has to include retransmission triggered both by a timer and by an ACK)

* integration with TLS 1.3 handshake - the basic 1-RTT mode is all that is needed here; exporters are not needed, nor are session tickets, but basic key exchange is sufficient, implementations can use any certificate; see TLS 1.3 for MTI cryptographic algorithms

* transport parameters - all the mandatory parameters and validation of version negotiation

* authentication for cleartext - FNV-1a authentication will be needed

* PADDING frames - to get the first packet to 1280 octets (policing of this limit at the server would also be good)

* connection ID - the server should pick a new connection ID (it's OK if it is the same as what the client chose, but it's hard to know that the server implemented the feature or that the client treats that properly if that happens)

This removes a bunch of things from consideration.  Implementations can do more of course, but we primarily want to validate that the basics work.  Not included:

* the entire HTTP mapping

* congestion control - the handshake can be assumed not to trigger the congestion controller, even in the face of loss

* packet protection - we will be done when the handshake is reported as successful

* flow control - we can assume that the flow control window is sufficiently open to handshake; max stream ID isn't needed because we won't have more than 1 stream

* streams other than the one carrying TLS