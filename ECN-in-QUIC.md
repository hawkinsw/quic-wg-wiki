# Introduction
ECN is short for Explicit Congestion Notification. 
ECN support in transport protocols is a fundamental feature that
should be included in the QUIC specification as a mandatory element.
ECN has the key benefit that it allows for non-destructive congestion
notification by a network node, i.e packets are marked instead
of being discarded.  This is particularly beneficial for realtime applications
with requirements on latency, ECN also has the benefit that it
provides with a congestion signal that is unambiguous (only congestion, not a 
transmission error) and that it is independent from the transport protocol. A transport
protocol can make use of ECN by writing one of the 2 ECN Capable Transport codepoints in 
the IP header. ECN capable networks will remark ECN capable packets with a Congestion 
Experienced codepoint. The transport protocol then MUST include an ECN-echo mechanism in 
its protocol, and respond to the echoed signal to drive it's congestion controller. The benefits
of ECN are described in more detail in [RFC8087](https://tools.ietf.org/html/rfc8087).
The ECN-echo functionality in QUIC should support both present and
future ECN, the latter is outlined in
[ECN experiments](https://www.rfc-editor.org/info/rfc8311), 
of particular interest is the
ability to discriminate between classic ECN and L4S ECN by means of
differentiation between the use of the ECT(0) and ECT(1) code points.
[ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) also covers this work, there may however be occasions where the wiki and the draft are not in synch, the ultimate goal is to move the specific wireformat and how-to´s to the QUIC transport draft later on.

# Requirements
* R.1 The ECN specification SHOULD be easy to implement. Care should be taken to minimize additional complexity in the implementation of ACK frames and the QUIC congestion control/recovery
* R.2 The ECN feedback MUST report all ECN-CE marked packets. This is important especially for the implementation of L4S support. It is an open question whether detailed packet information is needed or if is sufficient with a report of accumulated number of packets that are ECN-CE marked.
* R.3 The ECN feedback SHOULD report ECT(0) and ECT(1) marked packets. This is beneficial for the detection of remarking, bleaching or ECN black holes. 
* R.4 It SHOULD be possible measure amount of Not-ECT marked packets. This MAY be implemented as a dedicated feedback but can also be inferred from the regular ACK frames and the reports of CE, ECT(0) and ECT(1).
* R.5 A QUIC implementation may not be able to verify ECN support in the OS network stack. The capability check should cover this case similar as if the network does not support ECN (see R6).
* R.6 Initial connection ECN capability check SHOULD verify that ECN works e2e, this SHOULD be further verified at runtime 

# ECN capability check
The ECN capability check serves to:
1. Verify that the OS stacks in both endpoints support read and write of the ECN bits in the IP header.  
2. Initially test that ECN works over the used e2e network path.

ECN capability check start in the first packet that is transmitted from each of the two endpoints for a new path. This first packet sets ECT (ECT(0) or ECT(1)) in the IP header. The verification is two-way, i.e each direction is verified independently of the other. Given the possibility that the ECN capability check is successful in one direction but not the other (for instance due to ECN bleaching in one direction), means that ECN may only be used in one direction.

     ---------                                  ---------
     |       |---- 1st packet with ECT set ---->|       |
     |   A   |                                  |   B   |
     |       |<------ ACK + ECN counters -------|       |
     ---------                                  ---------


## Capability check, ect/echo
The capability check makes use of the generic ECN echo functionality of a receiver [consider explaining this first].
A peer (A) transmits the the first packet with the ECN bits set to ECT (either ECT(0) or ECT(1)). The specification of the ECT(0) and ECT(1) is as per the guidelines in [ECN experiments](https://www.rfc-editor.org/info/rfc8311)

The other peer (B) receives the packet and just performs the generic ECN echo functionality, i.e. it sends an ACK frame in return, that contains counters that indicates the amount of received packets with ECT(0),ECT(1) and CE. 

Peer A can confirm that the path direction supports ECN if the counters show a correct amount of packets received for a valid and expected counter combination. 

[In case duplicates are asked in QUIC: Once peer A receives the ACK frame it verifies that the number of ECT marked packets is equal to or greater than the number of transmitted ditto [ED note, "equal to or larger" in case duplicates are counted]
ECN capability check is deemed successful if the verification above yields a positive result, and ECN can be used for the given direction.

This capability check will verify that the path between the peers is free from issues with ECN bleaching and that the application does not have problems with access to the ECN bits in the IP header.       

## Handling of lost first frame
A lost 1st frame will be handled by QUICs retransmission logic, a retransmitted 1st frame should also have the ECT codepoint set.

## QUIC v1.0 limitation
Initial capability exchange and acking with ECN information MUST be implemented. 
After initial exchange the setting of ECT is OPTIONAL. If a sender sets the ECN field to ECT after the initial one the sender MUST implement a congestion response to ACKs indicating ECN-CE marked packets. It is expected that future QUIC profiles will define if ECN sender capability is mandated. 
   
# ECN feedback
The ECN feedback echoes the ECN marks back to the sender. The current assumption is that the ECN feedback should be in the same frame as the ACK. The main reason behind this is that it simplifies further processing in the congestion control and error recovery. A dedicated ACK_ECN frame should be used.

## ECN feedback, wire format
Following ECN block definition is proposed. The ECN block is appended after the ACK block section specified in [QUIC Transport](https://tools.ietf.org/wg/quic/draft-ietf-quic-transport/) 
The proposed format is useful both for classic ECN and L4S and encodes marked packets received. The DCTCP congestion control counts the number of marked bytes to calculate the drop probability (alpha), but [RFC7567](https://tools.ietf.org/html/rfc7567#section-4.4) states that ECN marking decisions should not take packet size into account, so packet counters will be sufficient, with a more compact ECN-echo protocol as a result.  

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

The ECT(0), ECT(1) and CE counters are in the worst case encoded with 8 octets each, this however assumes that all counters have very large values and a lot of packets have been sent in this connection.

## Reduction of overhead
Currently no header size optimization schemes are considered, besides the variable integers.
It is possible to reduce the size of the ECN block later by future protocol extensions.

A few observations on the properties of the counters outline the possibilities to reduce the overhead.
* The ECT(0) and ECT(1) counters are only needed to verify that the ECN bits are not cleared (ECN bleach) during the connection lifetime. For this reason it is sufficient to only report the 6 or 14 least significant bits of the ECT(0) and ECT(1) counters. Furthermore, either the ECT(0) or the ECT(1) is expected to be zero, given the recommended ECN bit usage in [ECN experiments](https://www.rfc-editor.org/info/rfc8311), this makes it possible to encode one ECT counter with only one octet.
* The CE counter only needs to record a sufficient number of bits to make it possible to compute a delta increase of the number of CE marked packets. Thus, it is sufficient to encode the CE counter with only 1 or 2 octets.

Given the above observations it should be possible to reduce the overhead to 5 octets, 1 octet for the "unused" ECT codepoint, 2 octets for the "used" ECT codepoints and 2 octets for the CE counter. Section 5.7 in [QUIC transport](https://quicwg.github.io/base-drafts/draft-ietf-quic-transport.html) outlines how packet numbers are safely encoded with only the least significant bits using, a similar approach is recommended for the ECN Block encoding.
It is however recommended that full counters are reported at CONNECTION_CLOSE and APPLICATION_CLOSEfor monitoring purposes. 

## Handling of lost ACKs
No special handling of lost ACK+ECN frames is necessary. The ECN counters are cumulative, which means that if an ACK+ECN frame with a potential increased CE count is lost, the next successfully received ACK+ECN frame will indicate the increased CE count.



# ECN support in various OS stacks
The network stack support for ECN varies between operating systems. In principle, what is needed is the ability to set and read the ECN bits in the IP header, from user space, in other words this access should preferably be possible without root privilege. 
## Linux 
ECN support is straightforward. 
On the sender side the code snippet below sets the ECN capability 

    int iptos = ect; 
    res = setsockopt(fd_out_rtp, IPPROTO_IP, IP_TOS,  &iptos, sizeof(iptos));

ect is 1 for ECT(0) and 2 for ECT(1)

On the receiver side it is necessary to use the recvmsg call with an extended struct to get access to the ECN bits. An example code snippet is found at [ECN in Linux](https://gist.github.com/jirihnidek/95c369996a81be1b854e).

## FreeBSD
ECN functions the same as for Linux
Ref. Lars Eggert

## Microsoft Windows
IP_RECVTOS (for IPv4) and IP_RECVTCLASS (for IPv6) socket option will allow a datagram socket to retrieve TOS bits including ECN values. See https://msdn.microsoft.com/en-us/library/windows/desktop/ms738586(v=vs.85).aspx. This API was added in Windows 10 Creators Update. No current documented way to set the ECN bits.

## Apple iOS, macOS, watchOS, etc.

To set ECT(0) or ECT(1) on outgoing UDP packets, use setsockopt() with IP_TOS (for IPv4) or IPV6_TCLASS (for IPv6).

To retrieve congestion marks from received UDP packets, use recvmsg() with IP_RECVTOS (for IPv4) or IPV6_RECVTCLASS (for IPv6).

## Android

TBD

# Suggested additions to 'to become' RFCs
This section outlines where the specification text for ECN in QUIC is suggested to be placed. (++) means that the section number is one more than the actual number, meaning that the section comes after..

## Transport draft ACK_ECN frame format

The following text on the ACK_ECN frame is suggested to be included in the transport draft

8.16++.  ACK_ECN Frame

The ACK_ECN frame is used to convey ACKs when the ECN capability exchange concludes that ECN should be used for the given connection. The ACK_ECN frame contains all the elements of the ACK frame with the addition of an ECN block appended at the end.

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

The ECN block is described below. The size (i) indicates variable-length encoding, explained in section 8.1 in [QUIC transport](https://quicwg.github.io/base-drafts/draft-ietf-quic-transport.html). The encoding length for each counter (1, 2, 4 or 8 bytes) should be selected such that all the significant bits are represented.

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

The receiver side should implement 3 64 bit counters that are copied to the ECN block when an ACK_ECN frame is generated. 
* ECT_0 : Initial value = 0, incremented when a packet marked ECT(0) is received
* ECT_1 : Initial value = 0, incremented when a packet marked ECT(1) is received 
* CE    : Initial value = 0, incremented when a packet marked CE is received 
Duplicate packets should not increment the counters

## Transport draft ECN capability exchange 

The following text on ECN capability exchange is suggested to be included in the transport draft

7.X ECN capability exchange

The capability check makes use of the ACK_ECN frame in section 8.16++. Each endpoint performs an ECN capability exchange in which the first packet sets the ECN bits in the IP header to either ECT(0) or ECT(1). The specification of the ECT(0) and ECT(1) is as per the guidelines in [ECN experiments](https://www.rfc-editor.org/info/rfc8311). Upon reception of the packet by the opposite peer, an ACK_ECN frame is transmitted back. This ACK_ECN frame indicates how many packets that are marked ECT(0), ECT(1) or CE.
   
The ACK_ECN frame will, when received, confirm that the path direction supports ECN if the counters show a correct amount of packets received for a valid and expected counter combination.
 
It is expected that QUIC discards duplicate packets early, however if that is not the case **[ED note, have not seen any clear statement in the drafts]**, then it should be verified that the number of ECT marked packets are equal to or larger that the amount of ECT marked packets that have been transmitted. 

ECT marked packets can become remarked as CE somewhere along the path between the peers, in such case the number of CE plus ECT marked packets should match the number of packets that was transmitted with ECT marking.     

ECN capability check is deemed successful if the verification above yields a positive result, and ECN can be used for the given direction. This capability check will verify that the path between the peers is free from issues with ECN bleaching and that the application does not have problems with access to the ECN bits in the IP header.       

## Recovery draft, ECN-CE reaction 

The following text is suggested in section 4.7 in the recovery draft. It addresses the reaction to ECN classic marking, i.e, the appropriate reaction when packets are marked ECT(0) as per the guidelines in [ECN experiments](https://www.rfc-editor.org/info/rfc8311).


***

The function OnAckReceipt() is motified to detect if it is an ACK frame or an ACK_ECN frame and to take the necessary additional actions for the case that it is an ACK_ECN frame

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
           largest_acked_packet
           OnPacketsMarked(ack.ce_counter, largest_acked_packet



***
An additional section that describes the OnPacketsMarked is added

4.7.6++.  On Packets Marked

      Invoked by an increment in the number of CE marked packets, as indicated by a newly received ACK_ECN frame.

      OnPacketsMarked(ce_counter):
        // Start a new congestion epoch
        if (end_of_recovery < largest_acked_packet):
          end_of_recovery = largest_sent_packet
          congestion_window *= kMarkReductionFactor
          congestion_window = max(congestion_window, kMinimumWindow)
          ssthresh = congestion_window
