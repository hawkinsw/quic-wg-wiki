Issue #2928 explains that in order to deal with "post quantum" crypto, we should be able to carry Client Hello
messages that are too large to fit in a single packet. To test support to that feature, we need to create
client hello packets that are larger than 1200 bytes. This can be achieved by sending a large Transport Parameter,
which for maximizing fun and confusion we call "Quantum Readiness". The transport parameter is defined as such:

* Parameter name: Quantum readiness
* Parameter code: 3127. (or any ID reserved for greasing)
* Parameter value: 1200 bytes all set to the value 'Q'

The client who wants to test Quantum readiness inserts the parameter in the transport parameter list.
Endpoints MUST ignore this parameter, as the transport parameter ID is the one reserved for greasing.

As an alternative, clients can send a large TP with an ID reserved for greasing, or split small ClientHello into multiple QUIC packets.