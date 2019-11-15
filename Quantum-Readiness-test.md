Issue #2928 explains that in order to deal with "post quantum" crypto, we should be able to carry Client Hello
messages that are too large to fit in a single packet. To test support to that feature, we need to create
client hello packets that are larger than 1200 bytes. This can be achieved by sending a large Transport Parameter,
which for maximizing fun and confusion we call "Quantum Readiness". The transport parameter is defined as such:

* Parameter name: Quantum readiness
* Parameter code: 3127. (This is a reserved value for tests)
* Parameter value: 1200 bytes all set to the value 'Q'

The client who wants to test Quantum readiness inserts the parameter in the transport parameter list.
Servers should ignore this parameter. Clients should ignore this parameter when decoding the Server hello message.

As an alternative, clients can send a large TP with an ID reserved for greasing, or split small ClientHello into multiple QUIC packets.