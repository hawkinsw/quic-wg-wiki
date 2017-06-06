This version of the implementation draft will concentrate on a successful handshake and a clean connection close, and will require integration with TLS 1.3. No application data will be exchanged. Implementing all of QUIC is difficult, if not impossible, at this stage, so we're going to focus on the 1RTT handshake. Specifically:

* Version negotiation - All server implementations will handle packets with an unknown version and arbitrary payloads and respond with a version negotiation packet. Client implementations will consume a version negotiation packet and either abort or select a compatible version. The expectation is that only one version will be implemented, but clients MAY implement greasing.

* Basic packetization and reliability. This includes:
  * Packetization for STREAM frames on Stream ID 0,
  * Transmitting those STREAM frames in unencrypted packets, 
  * Sending ACK frames in response (no timestamps are sent in unprotected packets)
  * Implementing a delayed ack timer is not necessary.
  * Processing ACK frames, removing acked packets from the retransmission buffer
  * Timer-based retransmission of unacknowledged handshake packets

  > Note: For this part, a sender does not have to measure RTT and can use a fixed timer for retransmitting handshake packets that haven't been acknowledged. A receiver must be able to handle packets received out of order and generate ACK frames that indicate missing packets.

* Basic STREAM sending and receiving. This includes:
  * Accepting stream data for the handshake and generating and sending STREAM frames.
  * Assembling a stream of bytes from received STREAM frames.

  > Note: For this part, a receiver must handle STREAM data received reordered, with gaps, or duplicated. Specifically, a receiver must allow for a sender to rebundle retransmitted STREAM data, meaning that a received STREAM frame may contain bytes that have already been received, and bytes that were lost.

* Integration with TLS 1.3 handshake - The basic 1-RTT mode must be supported. Transport parameter exchange is not needed, nor are session tickets.  Basic key exchange is sufficient and implementations can use any certificate.  TLS key export is necessary in order to exercise the 1-RTT keys.  All MTI algorithms listed in TLS 1.3 are expected.  Clients MUST offer a key share with P-256, servers MUST accept that share (this avoids the need for HelloRetryRequest).

* Authentication for cleartext - FNV-1a authentication will be needed.  Implementations will be expected to verify this and discard packets that are not correct.

* PADDING frames - The first client packet will be padded to 1280 octets (policing of this limit at the server would also be good).

* Connection ID - The server should pick a new connection ID.   It's OK if it is the same as what the client chose, but it's hard to know that the server implemented the feature or that the client treats that properly if that happens.  We want at least one implementation to pick a new connection ID.

* Packet protection - All post handshake packets must be sent with 1RTT keys and packet protection.  

* Connection Close - The implementation must close the connection after the handshake is complete.  This may be immediately or on a timer.  The peer must process the connection close packet and close the connection.  The connection close packet must use packet protection.  The packet containing a connection close does not need to be retransmitted.  Implementations should have configuration for switching this on, or for waiting for a close from their peer.

* Frame parsing for all frames - Rather than prohibit senders from generating frames that we don't expect to be used, receivers must accept all defined frame types.  Implementations need to parse every frame type, even if it is only so that they can correctly skip them.

This removes a bunch of things from consideration.  Implementations can do more of course, but we primarily want to validate that the basics work.  Not included:

* The entire HTTP mapping

* Congestion control - The handshake can be assumed not to trigger the congestion controller, even in the face of loss.

* Most of Loss recovery - The handshake is retransmitted via a fixed timer, but other forms of loss recovery are unnecessary.

* Flow control - We can assume that the flow control window is sufficiently open to handshake; max stream ID isn't needed because we won't have more than 1 stream.

* Streams other than the one carrying TLS.

* 0-RTT and resumption.

* Transport parameter exchange and negotiation.

* Key updates.

* Address validation and HelloRetryRequest.

* Public Reset.