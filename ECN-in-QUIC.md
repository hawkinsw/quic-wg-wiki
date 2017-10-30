# Introduction
ECN is short for Explicit Congestion Notification, this wiki page covers items such as requirements, [ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) and earlier versions give a short introduction to the topic. 

## Design team
A design team will focus of the wireline format of ECN capability exchange as well as the ECN feedback. It is suggested that the design team meet at IETF-100 to discuss the way forward and formalize a first joint version of the ECN specifics for QUIC. 

## Requirements
* R.1 The ECN specification SHOULD be easy to implement. Care should be taken to minimize additional complexity in the implementation of ACK frames and the QUIC congestion control/recovery
* R.2 The ECN feedback MUST report all ECN-CE marked packets. This is important especially for the implementation of L4S support. It is an open question whether detailed packet information is needed or if is sufficient with a report of accumulated number of bytes that are ECN-CE marked.
* R.3 The ECN feedback SHOULD report ECT(0) and ECT(1) marked packets. This is beneficial for the detection of remarking or ECN black holes. 
* R.4 It SHOULD be possible measure amount of Not-ECT marked packets. This MAY be implemented as a dedicated feedback but can also be inferred from the regular ACK frames and the reports of ECN, ECT(0) and ECT(1).
* R.5 A QUIC implementation MUST be able to verify ECN support in the OS network stack. 
* R.6 Initial ECN capability exchange SHOULD verify that ECN works e2e, this SHOULD be further verified at runtime 
* R.7 Initial ECN capability exchange MUST verify that endpoints support ECN, the ECN capability exchange MAY take place either at QUIC connection setup or after the connection setup is completed   

## ECN capability exchange
The ECN capability exchange serves to:
1. Verify that the OS stacks in both endpoints support read and write of the ECN bits in the IP header. This is simple and straightforward in Linux OS stacks. The level of support in Windows and OSX, iOS and Android is less clear.
2. Initially test that ECN works e2e 
### Proposed wire format
The wire format below is proposed for the ECN capability exchange. The capability exchange should take place after the connection setup. It may however take place already at the connection setup.

      0                   1
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |  Type         |C|R|W|U U U|E E|
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

The first byte contains a frame type, registered with IANA (?)
The second byte contains the flags:
* C: Challenge bit, indicates that the transmitted ECN negotiation frame is a challenge, if bit is not set then it is a response.
* R: Possible to read ECN bits in IP header
* W: Possible to write ECN bits in IP header
* EE : Echo of ECN bits
* U: Unused

The ECN negotiation has two steps:
* Challenge/response
* Determine degree of ECN support
### Capability exchange, challenge/response
   A peer transmits the ECN negotiation frame with the R,W and EE bits
   in the 2nd byte set to '0' and the C bit set to '1'.  This frame is
   echoed back with the flags set according to the degree of ECN support
   and with the ECN bits in the IP header of the received ECN
   negotiation frame copied to the EE field, the C bit is '0'.  As both
   peers MUST transmit an ECN negotiation frame there will be a total of
   4 ECN negotiation frames transmitted, two challenges and two
   responses.

   An ECN negotiation frame should be transmitted in a unique packet,
   this to avoid that possible loss of ECN negotiation packets cause
   loss of other frames than the ECN negotiation frame.

   The IP header for the ECN negotiation frame should set the ECN bits
   to CE '11'.  When the corresponding response is received then an EE
   pattern of '11' indicates that ECN is likely supported in the
   network.  This does not give a full guarantee that ECN is supported
   in the network.  Monitoring of the ECN field in the ACK-frame serves
   to give further indication of ECN support once ECN is turned on.

   An ECN negotiation is declared successful when an ECN negotiation
   response is received that indicates ECN support.  A peer is not
   allowed to set ECT on outgoing data packets until a successful ECN
   negotiation is done.  In other words it is only the ECN negotiation
   frame that is allowed to set the ECN bits in the IP header until ECN
   negotiation is concluded and successful.

   A lack of an ECN negotiation response may indicate that the ECN
   challenge frame or the ECN response frame was lost or that a node in
   the network deliberately discards ECN-CE marked packets.  The peer
   should transmit an additional ECN challenge within an RTO interval in
   case a negotiation response is not received, a maximum of
   retransmissions are attempted.

   A failed challenge/response phase indicates that ECN should not be
   used in the connection.  [NOTE, a special case is where one peer does
   not receive an ECN negotiation response but still receives ECT and CE
   marked packets from the other peer.  It is T.B.D how this should be
   handled]

### Capability exchange, determine degree of ECN support

   If the ECN challenge/response is successful, the degree of ECN
   capability depends on how the R, W and EE bits are set.

   * R='1' and EE= '11': It is possible to set the ECN bits in outgoing
      packets.

   * R='0' or EE <> '11': ECN support is not certain as it is either
      not possible for remote peer to read the ECN bits or that the ECN
      bits are altered.

   * W='1' : It is meaningful to send ECN feedback

   * W='0' : It is not meaningful to send ECN feedback as the remote
      peer cannot set (write) the ECN bits in the IP header.

   The mode mechanism in [RFC6679] can serve as in input to a solution
   for the support of ECN in the case that OS ECN support is asymmetric.
   It is however unclear how a QUIC implementation can determine
   asymmetric ECN support in the underlying OS.  For instance the method
   to send ECN marked packets to the local host to determine OS support
   does not reveal if the OS ECN support is asymmetric.

## ECN feedback
The ECN feedback echoes the ECN marks back to the sender. The exact details are subject to discussion. One proposal is found in section 2.3 in [ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt)
  