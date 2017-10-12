The original QUIC transport drafts, up to version -06, specified a possibility to encode time stamps as part of the ACK format. This time stamp specification did not see wide adoption -- most early implementations just set the number of included time stamps to zero. This outlined an underlying problem with the time stamp specification: usage of time stamps requires cooperation between sender and receivers. This cooperation requires agreement on a strategy on how and why to use time stamps. This agreement needs to be materialized by an explicit negotiation.

# One-way delays

A common use of timestamps is the measurement of one-way delays. Time based congestion protocol such as TCP-FAST, LEDBAT or BBR use the delay measurements as input: an increase in delay is indicative of increased queue sizes, and a decrease as a reduction in queuing. The precise RTT measurements enabled by the QUIC ACK can be used as input to these algorithms, but they lead to imprecision because an increase in RTT can be due to increase queuing in either the forward path or the reverse path. The use of time stamp allows for one-way delay measurements:

~~~
   ^    Data  ---\
   |              \---->
   Rtt1           /---- ACK1(Tr1)
   |             /
   V        <---/
   ^ Data    ---\
   |             \
   |              \--->
   Rtt2           /---- ACK2(Tr2)
   |             /
   V        <---/
   ^    Data  ---\
   |              \---->
   Rtt3            /--- ACK3(Tr3)
   |              /
   |             /
   V        <---/
~~~
In the illustrative example, both Rtt2 and Rtt3 are longer than Rtt1. In the absence of time stamps, 
the sender cannot distinguish between the two cases. In the presence of time stamps, the sender can measure
the difference between the sender local clock at the time of ACK reception and the time stamps. That
difference will not provide an absolute measurement of the one-way delay, but it will be enough to distinguish
between two signals. Then ACK2 is received, the difference between time stamp and local time is about the same
as when ACK1 was received, and the increase in RTT can be interpreted as an increase in delays on the
forward path. When ACK2 is received, the difference between time stamp and local time is larger
than when ACK1 or ACK2 were received, indicating that the RTT increase is mostly due to increased
delays on the reverse path.

## Protocol extensions for one way delay measurements

The one way delay measurements are enabled by negotiating the "One Way Delay" transport parameter option (code=TBD).
Clients indicate their willingness to measure one-way delays by including the option in their list of
initial parameters. Servers indicate their acceptance of the option by including it in their own list
in response. Servers MUST NOT include the option if it is not present in the client's list -- doing so would be 
a Protocol Error.

When the one-way delay option is negotiated, the format of ACK frames is changed to include the timestamp. The
timestamp is defined as the number of microseconds since the beginning of the connection, measured immediately
prior to sending the ACK frame. The least significant 32 bits of the timestamp are encoded on 4 bytes,
at the beginning of the ACK frame:
~~~
0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     ACK Time Stamp (32)                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|[Num Blocks(8)]|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                Largest Acknowledged (8/16/32/64)            ...
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|        ACK Delay (16)         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     ACK Block Section (*)                   ...
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

## Clock synchronization

The previous description assumes that the clocks at the sender and receiver are properly
synchronized. If they are not, the differences between sender and receiver clocks will
include both the path latency and the drift between the two clocks.

This specification does not include any protocol elements for performing
clock synchronization. Implementations should use appropriate filtering mechanisms
to estimate clock drift.

# Other use of time stamps

The one-way delay specification provides one estimate of the one way delay per received 
acknowledgement, i.e. typically multiple times per RTT. This satisfies the requirements
of time-based congestion control protocols, but it may not satisfy more
complex requirements, such as estimating the presence or not of delay compression, or
estimating the extent of out of order deliveries. Implementations interested in such
advance usage of time measurements should develop their own specification including
specific negotiation mechanisms.


