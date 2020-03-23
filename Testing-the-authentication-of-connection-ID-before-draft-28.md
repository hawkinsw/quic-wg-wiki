[PR #3499](https://github.com/quicwg/base-drafts/pull/3499/files) introduces two new transport parameters:

* `handshake_connection_id`, set by each peer as source CID of Initial packets
* `retry_connection_id`, set by a server that sent a Retry to the source CID of the Retry packet.

We expect that the PR will be included in draft-28, but it is useful to do some tests by adding the code now to at least some implementations of draft-27. Note that sending new transport parameters should not cause interop issues since implementations are supposed to ignore unknown TPs. The expectations for the early testing are:

1. Always send the `handshake_connection_id` TP.
2. Send the `retry_connection_id` TP when required by [PR #3499](https://github.com/quicwg/base-drafts/pull/3499/files).
3. Verify that the client is not sending `retry_connection_id` TP.
4. If the peer sent a `handshake_connection_id` TP, perform all the checks specified in [PR #3499](https://github.com/quicwg/base-drafts/pull/3499/files).
5. If receiving an Initial packet after sending a Retry, verify that the DCID matches the Retry CID.
6. If receiving `original_connection_id` from the server, verify that it matches even if no retry was received.

The goal is to get some early deployment and verify the PR.