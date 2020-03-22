PR #3499 introduces two new transport parameters:

* Handshake connection ID, set by each peer as source CID of Initial packets
* Retry CID, set by a server that sent a Retry to the source CID of the Retry packet.

We expect that the PR will be included in draft-28, but it is useful to do some tests by adding the code now to at least some implementations of draft 27. The expectations for the early testing are:

1. Always send the handshake CID TP. This should not cause an interop issue since implementations are supposed to ignore unknown TP.
2. Send the retry CID TP when also sending the ODCID.
3. Verify that the client is not sending Retry CID TP.
4. If negotiating draft 28 or later, or if the peer negotiated draft-27 and sent a "handshake CID" TP, perform all the checks specified in PR #349
5. If receiving an Initial packet after sending a Retry, verify that the DCID matches the Retry CID.

The goal is to get some early deployment and verify the PR.