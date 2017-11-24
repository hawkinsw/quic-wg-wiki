# Introduction
ECN is short for Explicit Congestion Notification. 
ECN support in transport protocols is a fundamental feature that
should be included in the QUIC specification as a mandatory element.
ECN has the key benefit that it allows for non-destructive congestion
notification by network node, i.e packets are marked instead
discarded.  This is particularly beneficial for realtime applications
with requirements on latency, ECN also has the benefit that it
provides with a congestion signal that is unambiguous.  The benefits
with ECN is described in more detail in [RFC8087](https://tools.ietf.org/html/rfc8087).
The ECN support should be implemented to support both present and
future ECN, the latter is outlined in
[ECN experiments](https://tools.ietf.org/wg/tsvwg/draft-ietf-tsvwg-ecn-experimentation/), of particular interest is the
ability to discriminate between classic ECN and L4S ECN by means of
differentiation between the use of the ECT(0) and ECT(1) code points.
[ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) also covers this work, there may however be occasions where the wike and the draft are not in synch, the ultimate goal is to move the specific wireformat and how-to´s to the QUIC transport draft later on.

# Design team
A design team will focus of the wireline format of ECN capability exchange as well as the ECN feedback. It is suggested that the design team meet at IETF-100 to discuss the way forward and formalize a first joint version of the ECN specifics for QUIC. 

# Requirements
* R.1 The ECN specification SHOULD be easy to implement. Care should be taken to minimize additional complexity in the implementation of ACK frames and the QUIC congestion control/recovery
* R.2 The ECN feedback MUST report all ECN-CE marked packets. This is important especially for the implementation of L4S support. It is an open question whether detailed packet information is needed or if is sufficient with a report of accumulated number of bytes that are ECN-CE marked.
* R.3 The ECN feedback SHOULD report ECT(0) and ECT(1) marked packets. This is beneficial for the detection of remarking, bleaching or ECN black holes. 
* R.4 It SHOULD be possible measure amount of Not-ECT marked packets. This MAY be implemented as a dedicated feedback but can also be inferred from the regular ACK frames and the reports of CE, ECT(0) and ECT(1).
* R.5 A QUIC implementation must not be able to verify ECN support in the OS network stack. The capability check should cover this case similar as if the network does not support ECN (see R6).
* R.6 Initial connection ECN capability check SHOULD verify that ECN works e2e, this SHOULD be further verified at runtime 

# ECN capability check
The ECN capability check serves to:
1. Verify that the OS stacks in both endpoints support read and write of the ECN bits in the IP header.  
2. Initially test that ECN works e2e 

ECN capability checks start in the first packet [ED note, should we say 'frame'?] that is transmitted from each of the two endpoints for a new path. This first packet sets ECT (ECT(0) or ECT(1)) in the IP header. The verification is two-way, i.e each direction is verified independently of the other. Given the possibility that the ECN capability check is successful in one direction but not the other (for instance due to ECN bleaching in one direction), means that ECN may only be used in one direction.

     ---------                                  ---------
     |       |-----1st packet with ECT set ---->|       |
     |   A   |                                  |   B   |
     |       |<-------ACK + ECN field-----------|       |
     ---------                                  ---------


## Capability check, challenge/response [or ect/echo?]
The capability check makes use of the generic ECN echo functionality of a receiver [consider explaining this first].
A peer (A) transmits the the first packet with the ECN bits set to ECT (either ECT(0) or ECT(1)). The specification of the ECT(0) and ECT(1) is as per the guidelines in [ECN experiments](https://tools.ietf.org/wg/tsvwg/draft-ietf-tsvwg-ecn-experimentation/)

The other peer (B) receives the packet and just performs the generic ECN echo functionality, i.e. it sends an ACK frame in return, that contains counters that indicates the amount of received bytes with ECT(0),ECT(1) and CE. 

Peer A can confirm that the path direction supports ECN if the counters show a correct amount of bytes received for a valid and expected counter combination. 

[In case duplicates are asked in QUIC: Once peer A receives the ACK frame it verifies that the number of ECT marked bytes(or packets) is equal to or greater than the number of transmitted ditto [ED note, "equal to or larger" in case duplicates are counted]
ECN capability check is deemed successful if the verification above yields a positive result, and ECN can be used for the given direction.]

This capability check will verify that the path between the peers is free from issues with ECN bleaching and that the application does not have problems with access to the ECN bits in the IP header.       

## Handling of lost first frame
A lost 1st frame will be handled by QUICs retransmission logic, a retransmitted 1st frame should also have the ECT codepoint set.

## QUIC v1.0 limitation
Subsequent packets (after the 1st) should set the ECN bits to Not-ECT. This because v1.0 will only verify that the ECN capability exchange and the ACK+ECN frame operates correctly.  

# ECN feedback
The ECN feedback echoes the ECN marks back to the sender. The current assumption is that the ECN feedback should be in the same frame as the ACK. The main reason behind this is that it simplifies further processing in the congestion control and error recovery.

At the QUIC interrim (October 2017) it was decided that timestamps should be removed from the default ACK frames, this means that ECN frames are unlikely to be in the default ACK frames as well. Therefore a dedicated ECN+ACK frame is needed. [Since ECN echo and ASKs are tightly coupled it might be better to use the same frame. It will reduce the ACK size as it will always be together with the ACK.]
While timestamps and ECN may not be tightly coupled, there is a possibility that the two can be combined for enhanced congestion control purposes. Two examples are 
1. [SCReAM](https://tools.ietf.org/wg/rmcat/draft-ietf-rmcat-scream-cc/) seamlessly combines ECN/loss feedback and delay estimation, thus delay estimation serves as a fallback for the case that congested nodes are non ECN capable.
2. BBR type bandwidth estimation may be combined with L4S marking for improved MIMD type congestion control, suitable for e.g high bitrate interactive applications such as VR.

This leads to a few questions when a combined ECN+ACK frame is devised.
1. Should it be an ECN+ACK frame only ?
2. Should it be an ECN+ACK+TS frame ?

An additional question is if the ECN field should report 
1. Bytes marked
2. Packets marked

The wire specification below only address the ACK+ECN frame and assumes bytes marked as report metric.
 
## ECN feedback, wire format
[TODO rewrite this in its entirety, a more compressed format is highly recommended]

The proposed alternative proposes a format for an ECN block. The ECN block is appended after the ACK block section specified in [QUIC Transport](https://tools.ietf.org/wg/quic/draft-ietf-quic-transport/) 
The proposed format is useful both for classic ECN and L4S. 

      0                   1                   2                   3
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |n m: # ECT(0) marked bytes encoded as 6,14,30 or 62 bits   ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |n m: # ECT(1) marked bytes encoded as 6,14,30 or 62 bits   ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     |n m: # ECN-CE marked bytes encoded as 6,14,30 or 62 bits   ...
     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
This is encoded as 1,2,4 or 8 octet each, given by the most significant [nm] bits for each field
as:


* nm=00: 1 octet,  giving 6 bits for the encoding
* nm=01: 2 octets, giving 14 bits for the encoding
* nm=10: 4 octets, giving 30 bits for the encoding
* nm=11: 8 octets, giving 62 bits for the encoding

The proposed encoding enables flexible and compact encoding of the ECN
information, with a minimal 1 octet overhead per counter. 
The marked bytes counted are including QUIC header and payload but excluding UDP and IP headers.
In normal ECN operation it is likely that only the ECN-CE bytes field, and either of the ECT(0) or ECT(1) bytes fields are encoded. The number of bits to encode can also be used wisely to make for efficient encoding.

[ED note, 6 bits to encode number of marked bytes is likely quite useless, unless of course the counter does not change at all] 

# ECN support in various OS stacks
The network stack support for ECN varies between operating systems. In principle, what is needed is the ability to set and read the ECN bits in the IP header, from user space, in other words this access should preferably be possible without root privilege. 
## Linux 
ECN support is straightforward. 
On the sender side the code snippet below sets the ECN capability 

    int iptos = ect; 
    res = setsockopt(fd_out_rtp, IPPROTO_IP, IP_TOS,  &iptos, sizeof(iptos));

ect is 1 for ECT(0) and 2 for ECT(1)

On the receiver side it is necessary to use the recvmsg call with an extended struct to get access to the ECN bits. An example code snippet is found at [ECN in Linux](https://gist.github.com/jirihnidek/95c369996a81be1b854e).

## Microsoft Windows
IP_RECVTOS (for IPv4) and IP_RECVTCLASS (for IPv6) socket option will allow a datagram socket to retrieve TOS bits including ECN values. See https://msdn.microsoft.com/en-us/library/windows/desktop/ms738586(v=vs.85).aspx. This API was added in Windows 10 Creators Update. No current documented way to set the ECN bits.

## Apple iOS, macOS, watchOS, etc.

To set ECT(0) or ECT(1) on outgoing UDP packets, use setsockopt() with IP_TOS (for IPv4) or IPV6_TCLASS (for IPv6).

To retrieve congestion marks from received UDP packets, use recvmsg() with IP_RECVTOS (for IPv4) or IPV6_RECVTCLASS (for IPv6).

## Android

TBD
