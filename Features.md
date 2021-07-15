This wiki outlines the set of various features supported by at least some of the available implementations. Please note that this isn't a 1 to 1 map of features tested for [interop](21st-Implementation-Draft) because many of those are fundamental requirements to claim support at all. This list is just for the set of features that aren't always implemented by all.

Some of the above features might only be supported for the client or server roles. In this case, the feature tag should be post-fixed with either `-client` or `-server` as appropriate. For instance, if 0-RTT is only supported on the server side, then the tag would be `0-rtt-server`.

# QUIC Features

QUIC protocol/implementation specific features.

- `client` - Supports the client role/side of a connection.

- `server` - Supports the server role/side of a connection. This includes things like creating a "listener" to accept incoming connections.

- `resumption` - Supports TLS session tickets and using them to resume a previous TLS session.

- `0-rtt` - Supports exchanging application data secured with 0-RTT keys.

- `nat` - Supports NAT rebinding and all necessary path validation logic.

- `migration` - Supports explicit path migration by the client.

- `cubic` - Supports the cubic congestion control algorithm.

- `bbr` - Supports the BBR v1 congestion control algorithm.

- `bbr2` - Supports the BBR v2 congestion control algorithm.

- `ecn-recv` - Supports receiving and acknowledging ECN marked QUIC packets.

- `ecn-send` - Supports reacting to ECN acknowledgements in congestion control logic.

- `priority` - Supports explicit stream prioritization signals for send ordering across streams.

- `ver-neg` - Supports the version negotiation extension.

- `datagram` - Supports the unreliable datagram extension.

- `ack-freq` - Supports the ACK frequency extension.

- `lb` - Supports at least some form of QUIC based load balancing.

# HTTP/3 Features

HTTP/3 protocol/implementation specific features.

- `h3-client` - Supports the client role/side of a connection.

- `h3-server` - Supports the server role/side of a connection. This includes things like creating a "listener" to accept incoming connections.
