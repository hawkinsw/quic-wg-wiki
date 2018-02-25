# Introduction
Explicit Congestion Notification ECN) support in transport protocols 
is a fundamental feature that
should be included in the QUIC specification as a mandatory element.
ECN has the key benefit that it allows for non-destructive congestion
notification by a network node, i.e packets are marked instead
of being discarded by routers and other devices along a network path.  
This is particularly beneficial for realtime applications
with requirements on latency, ECN also has the benefit that it
provides with a congestion signal that is unambiguous (only congestion, not a 
transmission error) and that it is independent from the transport protocol.
The benefits
of ECN are described in more detail in [RFC8087](https://tools.ietf.org/html/rfc8087).

A transport
protocol can make use of ECN by writing one of the 2 ECN Capable Transport codepoints in 
the IP header. ECN capable networks are then allowed to remark ECN capable packets to a Congestion 
Experienced (CE) codepoint. The transport protocol then MUST include an ECN-echo mechanism in 
its protocol, and respond to the echoed signal to drive its congestion controller. 
The ECN-echo functionality in QUIC should support both present and
future ECN, the latter is outlined in
[ECN experiments](https://www.rfc-editor.org/info/rfc8311), 
of particular interest is the
ability to discriminate between classic ECN and L4S ECN by means of
differentiation between the use of the ECT(0) and ECT(1) code points.
[ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) also covers this work, there may however be occasions where the wiki and the draft are not in synch, the ultimate goal is to move the specific wireformat and how-to´s to the QUIC transport draft later on.

# Requirements
The folloing requirements are placed on a transport supporting ECN:
* R.1 The ECN specification SHOULD be easy to implement. Care ought be taken to minimize additional complexity in the implementation of ACK frames and the QUIC congestion control/recovery.
* R.2 The ECN feedback MUST report all ECN-CE marked packets. This is important for any ECN reaction that is based on the rate or pattern of marking (including L4S support). It is an open question whether detailed packet information is needed or if it is sufficient to report the accumulated number of packets that are ECN-CE marked.
* R.3 The ECN feedback SHOULD report the number of packets with the ECT(0) and ECT(1) codepoint. This is beneficial for the detection of remarking, bleaching or ECN black holes. 
* R.4 It SHOULD be possible measure amount of Not-ECT marked packets. This MAY be implemented as a dedicated feedback but can also be inferred by combining the contents of regular ACK frames with the reports for CE, ECT(0) and ECT(1).
* R.5 Initial connection ECN capability check SHOULD verify that ECN works e2e, this SHOULD be further verified at runtime 
* R.6 Some QUIC implementations may be unable to verify ECN support in the OS network stack. The outcome of the ECN capability check SHOULD be the same as the case when network does not support ECN (see R5).


# ECN capability check
The ECN capability check is unidirectional. That is, the check is independently performed for each direction of transmission.

The ECN capability check serves to:
1. Verify that the OS stack at the sending endpoint can write to the ECN field in the IP header.  
1. Verify that the OS stack at the receiving endpoint can read from the ECN field in the IP header.  
2. Initially test that ECN works over the used e2e network path.

The ECN capability check starts with the first packet that is transmitted from each endpoint along a new path. This first packet sets the ECT field in the IP header to a avlue of ECT(0) or ECT(1).  The ECN capability check can be successful in one direction, but not in the other (for instance due to ECN bleaching [RFC8087](https://tools.ietf.org/html/rfc8087) in one direction). This could result in ECN only being used in one of the directions.

     ---------                                  ---------
     |       |---- 1st packet with ECT set ---->|       |
     |   A   |                                  |   B   |
     |       |<------ ACK + ECN counters -------|       |
     ---------                                  ---------

## ECN Feedback
The ECN Feedback function echoes the value of ECN marks of received packets back to the sender. The current assumption is that the ECN feedback should be in the same frame as the ACK. The main reason behind this is that this simplifies further processing in the congestion control and error recovery. A dedicated ACK_ECN frame should be used.

## Capability Check, ECT/echo
The capability check uses the ECN Feedback function to verify that one direction of the path between the endpoints is free from issues with ECN bleaching and that the application does not have problems with access to the ECN field in the IP header.       

The sending endpoint (A) transmits the the first packet with the ECN field set to ECT (either ECT(0) or ECT(1)). The usage of the ECT(0) and ECT(1) is defined in [ECN experiments](https://www.rfc-editor.org/info/rfc8311)

The remote endpoint (B) receives the packet and performs the generic ECN echo functionality, i.e. it sends an ACK frame in the return direction to the sendind endpoint (A). This contains counters that indicates the amount of received packets with ECT(0), ECT(1) and CE. 

Reception of the ECN Feedback by the senidng endpoint (A) allows the endpoint to evaluate whether the path in the forward direction was ECN-capable. This is confirmed when the reported ECN counters show a correct amount of packets received for a valid and expected counter combination. 

[In case duplicates are asked in QUIC: Once endpoint A receives the ACK frame it verifies that the number of ECT marked packets is equal to or greater than the number of transmitted ditto [ED note, "equal to or larger" in case duplicates are counted]

The ECN capability check is deemed successful if the verification above yields a positive result.

## Handling of a lost first frame
Loss of the 1st frame will be handled by the QUIC retransmission logic. A retransmitted 1st frame SHOULD set an ECT codepoint.

## QUIC v1.0 Limitation

An implementation is REQUIRED to support the capability check and provide the generic ECN echo function.

An ECN-capable sender is REQUIRED to implement a congestion response when it receives ACKs indicating reception of ECN-CE marked packets by the remote endpoint. After completing the capability check, this sender MAY sets the ECN field to ECT after the initial capability check.

A sender that does not set the ECN field to ECT, ought to send packets with a Not ECT codepoint in the ECN field.

It is expected that future QUIC profiles will define if ECN sender capability is mandated. 
   
## ECN feedback, wire format
The following ECN block definition is proposed. The ECN block is appended after the ACK block section specified in [QUIC Transport](https://tools.ietf.org/wg/quic/draft-ietf-quic-transport/).

The proposed format encodes marked packets received. It is useful both for Classic ECN and L4S. The DCTCP congestion control counts the number of marked bytes to calculate the drop probability (alpha), but [RFC7567](https://tools.ietf.org/html/rfc7567#section-4.4) states that ECN marking decisions should not take packet size into account, so packet counters will be sufficient, with a more compact ECN-echo protocol as a result.  

      0                   1                   2                   3
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECT(0) marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECT(1) marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECN-CE marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

(i) indicates variable-length encoding, explained in section 8.1 in [QUIC transport](https://quicwg.github.io/base-drafts/draft-ietf-quic-transport.html)

The ECT(0), ECT(1) and CE counters are in the worst case encoded with 8 octets each, this however assumes that all counters have large values corrsponding to many packet been sent using this connection.

## Reduction of overhead
Currently no header size optimization schemes are considered, besides using a variable integer format.
It is possible to reduce the size of the ECN block in future protocol extensions.

From observations of the properties of the counters, it is possible to outline the possibilities to reduce the overhead:
* The ECT(0) and ECT(1) counters are only needed to verify that the ECN bits are not cleared (ECN bleach) during the connection lifetime. For this reason it is sufficient to only report the 6 or 14 least significant bits of the ECT(0) and ECT(1) counters. Furthermore, either the ECT(0) or the ECT(1) counter is expected to be zero, given the recommended ECN bit usage in [ECN experiments] (https://www.rfc-editor.org/info/rfc8311), this makes it possible to encode one ECT counter with only one octet.
* The CE counter only needs to record a sufficient number of bits to make it possible to compute a delta increase of the number of CE marked packets. Thus, it is sufficient to encode the CE counter with only 1 or 2 octets.

Given the above observations, it ought to be possible to reduce the overhead to 5 octets, 1 octet for the "unused" ECT codepoint, 2 octets for the "used" ECT codepoints and 2 octets for the CE counter. Section 5.7 in [QUIC transport](https://quicwg.github.io/base-drafts/draft-ietf-quic-transport.html) outlines how packet numbers are safely encoded with only the least significant bits using, a similar approach is recommended for the ECN Block encoding.
It is however recommended that full counters are reported at CONNECTION_CLOSE and APPLICATION_CLOSE for monitoring. 

## Handling of lost ACKs
No special handling of lost ACK+ECN frames is necessary. The ECN counters are cumulative. This means that if an ACK+ECN frame with a potential increased CE count is lost, the next successfully received ACK+ECN frame will indicate the increased CE count.

# ECN support in various OS stacks
The network support for ECN varies between operating system stacks. This mechanism needs to have the ability to set and read the ECN field in the IP header, from user space, in other words this access should preferably be possible without root privilege. 

## Linux 
At the sending endpoint, the code snippet below sets the ECN capability: 

    int iptos = ect; 
    res = setsockopt(fd_out_rtp, IPPROTO_IP, IP_TOS,  &iptos, sizeof(iptos));

ect is 1 for ECT(0) and 2 for ECT(1)

At the receiver endpoint, it is necessary to use the recvmsg call with an extended struct to access to the ECN field. An example code snippet is found at [ECN in Linux] (https://gist.github.com/jirihnidek/95c369996a81be1b854e).

## FreeBSD
ECN functions the same as for Linux.

Ref. Lars Eggert

## Microsoft Windows

There is no current documented way to set the ECN bits.

IP_RECVTOS (for IPv4) and IP_RECVTCLASS (for IPv6) socket option will allow a datagram socket to retrieve TOS bits including ECN values. See https://msdn.microsoft.com/en-us/library/windows/desktop/ms738586(v=vs.85).aspx. 

This API was added in Windows 10 Creators Update.

## Apple iOS, macOS, watchOS, etc.

At the sending endpoint, the ECN field can be set on outgoing UDP packets, use setsockopt() with IP_TOS (for IPv4) or IPV6_TCLASS (for IPv6).

At the receiver endpoint, the ECN field of received UDP packets can be read using recvmsg() with IP_RECVTOS (for IPv4) or IPV6_RECVTCLASS (for IPv6).

## Android

TBD

# Suggested additions to 'to become' RFCs

This section outlines where the specification text for ECN in QUIC is suggested to be placed. (++) means that the section number is one more than the actual number, meaning that the section comes after..

## Transport draft: ACK_ECN frame format

The following text on the ACK_ECN frame is suggested to be included in the transport draft:

8.16++.  ACK_ECN Frame

A QUIC connection MUST keep counters for each ECN codepoint, recording the number of packets that were received with the corresponding ECN codepoint in the IP header. If the header is not readable from the application, the codepoint 00 MUST be assumed. 

The ACK_ECN frame is used by the reeiver to echo the value of these counters back to the sender of these packets. This allows the sender to utilize these counter values for congestion control. The ACK_ECN frame contains all the elements of the ACK frame with the addition of an ECN block appended at the end.

     0                   1                   2                   3
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |                     Largest Acknowledged (i)                ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |                          ACK Delay (i)                      ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |                       ACK Block Count (i)                   ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |                          ACK Blocks (*)                     ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |                           ECN Block                         ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                  Figure NN1: ACK_ECN Frame Format
 
8.16++.1.  ECN Block

The ECN block is described below. The size (i) indicates variable-length encoding, explained in section 8.1 in [QUIC transport] (https://quicwg.github.io/base-drafts/draft-ietf-quic-transport.html). The encoding length for each counter (1, 2, 4 or 8 bytes) should be selected such that all the significant bits are represented.

      0                   1                   2                   3
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECT(0) marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECT(1) marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |# ECN-CE marked packets (i)                                 ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                  Figure NN2: ECN Block

8.16++.2   ECN counters

The receiver side should implement three 64-bit counters that are copied to the ECN block when an ACK_ECN frame is generated. 
* ECT_0 : Initial value = 0, incremented when a packet marked ECT(0) is received
* ECT_1 : Initial value = 0, incremented when a packet marked ECT(1) is received 
* CE    : Initial value = 0, incremented when a packet marked CE is received 

Reception of duplicate packets SHOULD NOT increment the counters.

## Transport draft: ECN capability check

The following text on ECN capability check is suggested to be included in the transport draft:

7.X ECN capability check

The capability check makes use of the ACK_ECN frame in section 8.16++. Each endpoint performs an ECN capability check in which the first packet, containing the 1st frame [is this a necessary condition, or can this be removed?], SHOULD set the ECN bits in the IP header to either ECT(0) or ECT(1). The specification of the ECT(0) and ECT(1) is as per the guidelines in [ECN experiments](https://www.rfc-editor.org/info/rfc8311). Upon reception of the packet by the opposite peer, an ACK_ECN frame is transmitted back. This ACK_ECN frame indicates how many packets that are marked ECT(0), ECT(1) or CE. 

A retransmitted 1st frame SHOULD also set the ECT codepoint.
 
[ED note. Not full agreement in the design team, an option is that the ECT code point is not set for the retransmitted 1st frame, ECT black holes are still a theoretical possibility even though recent studies have indicated that they are very rare, it is for instance more likely that the 1st packet is dropped due to a UDP black hole] 
   
The ACK_ECN frame will, when received, confirm that the path direction supports ECN if the counters show a correct amount of packets received for a valid and expected counter combination.
 
It is expected that QUIC discards duplicate packets early, however if that is not the case **[ED note, have not seen any clear statement in the drafts]**, then it should be verified that the number of ECT marked packets are equal to or larger that the amount of ECT marked packets that have been transmitted. 

ECT marked packets can become remarked as CE along the path between the peers, in such a case the number of CE plus ECT marked packets should match the number of packets that was transmitted with ECT marking.     

[[ Note: the equality has to be evaluated in a way robust to network reordering of IP packets]]

The ECN capability check is deemed successful if the verification above yields a positive result, and then ECN can be used for the given direction. This capability check will verify that one direction of the path between the peers is free from issues with ECN bleaching and that the application does not experience problems with access to the ECN field in the IP header.       

## Transport draft: Connection migration

ECN capability MUST be verified at connection migration, this to verify that both endpoints support ECN and that the path remains free from ECN bleaching. An additonal section is suggested in the transport draft.

7.7.3.  ECN capability check for Migrated Connection
Connection migration requires that ECN capability is verified again. 

The ECN capability as indicated in section 7.X should be repeated when a connection is migrated. This verifies that the endpoints are ECN-capable and that the ECN field is not bleached along the new path.
There are two unique cases:
1. ECN capability check successful at initial connection setup
2. ECN capability check failed at initial connection setup

Case 1 can have different outcomes, either that ECN capability will continue, or that ECN capability is turned off. Case 2 means that ECN capability can be enabled after a connection even though it was disabled at the initial connection setup. 
The two cases are descibed more in detail below. 
[ED note.. unsure is a new connection really instantiated at connection migration ?] Connection migration has impact on the number of reported CE marked packets. A new connection state with new congestion control state and ECN counters is instantiated at the sender and receiver. The ECN counters MUST start from zero again.

7.7.3.1 Case 1, ECN capability check successful at initial connection setup

IP packets continue to be transmitted with the applicable ECT code point. One possible issue is that ECN does not work along the new path, either because of ECN bleaching in the network or because of OS network stack issues. This error case will manifest itself in that the ECT and CE counters do not increase as much as they should. The sender detects this and consequently ECN capability is disabled. The trigger condition for a successful ECN capability check is the same as for the intial ECN capability check. 

7.7.3.2 Case 2, ECN capability check failed at initial connection setup

The applicable ECT codepoint is set in the IP packets along the new connection and the ECN capability check as outlined in section 7.x is initiated.

## Recovery draft: ECN-CE reaction 

The following additional text is suggested in the recovery draft. It addresses the reaction to ECN classic marking, i.e, the appropriate reaction when packets are marked ECT(0) as per the guidelines in [ECN experiments](https://www.rfc-editor.org/info/rfc8311).


***

The function OnAckReceived() is modified to detect if it is an ACK frame or an ACK_ECN frame and to take the necessary additional actions for the case that it is an ACK_ECN frame

3.4.5.  On Ack Receipt

      OnAckReceived(ack):           
        largest_acked_packet = ack.largest_acked
        // If the largest acked is newly acked, update the RTT.
        if (sent_packets[ack.largest_acked]):
          latest_rtt = now - sent_packets[ack.largest_acked].time
          UpdateRtt(latest_rtt, ack.ack_delay)
        // Find all newly acked packets.
        for acked_packet in DetermineNewlyAckedPackets():
          OnPacketAcked(acked_packet.packet_number)

        DetectLostPackets(ack.largest_acked_packet)
        SetLossDetectionAlarm()
        // Detect if ACK_ECN frame indicates ECN marking
        if (ack is of type ACK_ECN):
           OnPacketsMarked(ack.ce_counter)



***
An additional section that describes the OnPacketsMarked is added

4.7.6++.  On Packets Marked

      Invoked by an increment in the number of CE marked packets, as indicated by a newly received ACK_ECN frame. 
      The variable ack_ce_counter is used to check if packets are recently CE marked

      OnPacketsMarked(ce_counter):
        if (end_of_recovery < largest_acked_packet && ce_counter > ack_ce_counter):
          // Start a new congestion epoch
          end_of_recovery = largest_sent_packet
          congestion_window *= kMarkReductionFactor
          congestion_window = max(congestion_window, kMinimumWindow)
          ssthresh = congestion_window

        // update ack_ce_counter
        ack_ce_counter = ce_counter