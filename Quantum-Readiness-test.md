[Issue #2928](https://github.com/quicwg/base-drafts/issues/2928) explains that in order to deal with "post quantum" crypto, we should be able to carry Client Hello
messages that are too large to fit in a single packet. To test support to that feature, we need to create
client hello packets that do not fit in one packet. This can be achieved by sending a large Transport Parameter. [We've reserved 0x173E for this purpose](https://github.com/quicwg/base-drafts/wiki/Temporary-IANA-Registry). The transport parameter is defined as such:

* Parameter name: discard
* Parameter code: 0x173E
* Parameter value: anything, it's discarded!

All lengths are allowed for this transport parameter, including zero.

The client who wants to test Quantum readiness inserts the parameter in the transport parameter list.
Endpoints that receive this parameter MUST ignore it.

As an alternative, clients can send a large TP with an ID reserved for greasing, or split small ClientHello into multiple QUIC packets.

All credit for the discard idea goes to [RFC 863](https://tools.ietf.org/html/rfc863).