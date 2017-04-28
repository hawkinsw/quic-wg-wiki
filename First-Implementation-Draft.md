This version of the implementation draft will concentrate on just the handshake, but will require integration with TLS 1.3. Implementing all of QUIC is difficult, if not impossible, at this stage, so we're going to look more closely at the basic handshake only. Specifically:

* version negotiation - All server implementations will handle packets with an unknown version and arbitrary payloads and respond with a version negotiation packet. Client implementations will consume a version negotiation packet and either abort or select a compatible version. The expectation is that only one version will be implemented, but clients MAY implement greasing.

* basic packetization and reliability. This includes:
  * packetization for STREAM frames on Stream ID 1,
  * transmitting those STREAM frames in unencrypted packets, 
  * sending ACK frames in response (no timestamps needed)
  * processing ACK frames, removing acked packets from the retransmission buffer
  * timer-based retransmission of lost handshake packets

  > Note: For this part, a sender does not have to generate RTT samples and can use a fixed timer for retransmitting lost handshake packets. A receiver must be able to handle packets received out of order and generate ACK frames that indicate missing packets.

* basic STREAM sending and receiving. This includes:
  * accepting stream data for the handshake and generating STREAM frames
  * assembling a stream of bytes from received STREAM frames.

  > Note: For this part, a receiver must handle STREAM data received reordered, with gaps, or duplicated. Specifically, a receiver must allow for a sender to rebundle retransmitted STREAM data, meaning that a received STREAM frame may contain bytes that have already been received, and bytes that were lost.

* integration with TLS 1.3 handshake - The basic 1-RTT mode must be supported. TLS exporters are not needed, nor are session tickets.  Basic key exchange is sufficient and implementations can use any certificate.  All MTI algorithms listed in TLS 1.3 are expected.

* HelloRetryRequest - This need not be stateless, but implementations need to support HelloRetryRequest in TLS for other reasons. Address validation may be supported.

* transport parameters - This includes sending and parsing all the mandatory parameters. Implementations will also have to validate version negotiation and fail the handshake if that is not successful.

* authentication for cleartext - FNV-1a authentication will be needed.  Implementations will be expected to verify this and discard packets that are not correct.

* PADDING frames - The first client packet will be padded to 1280 octets (policing of this limit at the server would also be good).

* connection ID - The server should pick a new connection ID.   It's OK if it is the same as what the client chose, but it's hard to know that the server implemented the feature or that the client treats that properly if that happens.  We want at least one implementation to pick a new connection ID.

* frame parsing for all frames - Rather than prohibit senders from generating frames that we don't expect to be used, receivers must accept all defined frame types.  Implementations need to parse every frame type, even if it is only so that they can correctly skip them.

This removes a bunch of things from consideration.  Implementations can do more of course, but we primarily want to validate that the basics work.  Not included:

* the entire HTTP mapping

* congestion control - The handshake can be assumed not to trigger the congestion controller, even in the face of loss.

* packet protection - We will be done when the handshake is reported as successful.

* flow control - We can assume that the flow control window is sufficiently open to handshake; max stream ID isn't needed because we won't have more than 1 stream.

* streams other than the one carrying TLS