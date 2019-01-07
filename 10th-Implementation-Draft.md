* Record interop results at [here](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit#gid=11370678)

* -17

With draft-17 (v=ff000011), test the following features:


|Feature | code | details  |
|--------------------|:---:|------------------------|
|Version Negotiation | V | A version negotiation response is elicited and acted on |
|Handshake | H | The handshake completes successfully |
|Stream Data | D | Stream data is being exchanged and ACK'ed |
|Connection Close | C | The connection close procedure completes w/a zero error code |
|Resumption | R | Connection is established using TLS Resume Ticket |
|0-RTT | Z | 0-RTT data is being sent and acted on |
|Stateless Retry | S | A handshake that includes a Retry packet completes successfully |
|Migration | M | A new CID is offered to the peer, and it migrates the connection to it |
|Rebinding | B | The client moves to a different address or port, and the server migrates to it |
|Key Update | U | One endpoint can update keys and its peer responds correctly |
|H3 | 3 | An H3 transaction succeeded |

