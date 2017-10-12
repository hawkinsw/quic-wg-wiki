# Introduction
ECN is short for Explicit Congestion Notification, this wiki page covers items such as requirements, [ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) and earlier versions give a reasonable  

## Design team
A design team will focus of the wireline format of ECN capability exchange as well as the ECN feedback. It is suggested that the design team meet at IETF-100 to discuss the way forward and formalize a first joint version of the ECN specifics for QUIC. 

## Requirements
* R.1 The ECN specification SHOULD be easy to implement. Care should be taken to avoid additional complexity in the implementation of ACK frames and the QUIC congestion control/recovery
* R.2 The ECN feedback MUST report all ECN-CE marked packets. This is important especially for the implementation of L4S support. It is an open question whether detailed packet information is needed or if is sufficient with a report of accumulated number of bytes that are ECN-CE marked.
* R.3 The ECN feedback SHOULD report ECT(0) and ECT(1) marked packets. This is beneficial for the detection of remarking or ECN black holes. 
* R.4 It SHOULD be possible measure amount of Not-ECT marked packets. This MAY be implemented as a dedicated feedback but can also be inferred from the regular ACK frames and the reports of ECN, ECT(0) and ECT(1).
* R.5 A QUIC implementation MUST be able to verify ECN support in the OS network stack. 
* R.6 Initial ECN capability exchange SHOULD verify that ECN works e2e, this SHOULD be further verified at runtime 
* R.7 Initial ECN capability exchange MUST verify that endpoints support ECN   

## ECN capability exchange
The ECN capability exchange serves to:
1. Verify that the OS stacks in both endpoints support read and write of the ECN bits in the IP header. This is simple and straightforward in Linux OS stacks. The level of support in Windows and OSX, iOS and Android is less clear.
2. Initially test that ECN works e2e 
Section 2.1 in [ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt) contains a proposal for ECN capability exchange.

## ECN feedback
The ECN feedback echoes the ECN marks back to the sender. The exact details are subject to discussion. One proposal is found in section 2.3 in [ECN draft](https://tools.ietf.org/id/draft-johansson-quic-ecn-03.txt)
  