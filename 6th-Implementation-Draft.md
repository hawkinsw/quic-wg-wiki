Same as 5th draft for the tests, but implementing draft-12 and [PR #1389](https://github.com/quicwg/base-drafts/pull/1389).

There is a new "migration" test, which isn't terribly well defined at the moment. The idea is that if the peer sent a `NEW_CONNECTION_ID` frame, the local endpoint would start using that new CID for the connection, and verifies that that worked.