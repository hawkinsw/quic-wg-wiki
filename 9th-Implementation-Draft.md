* Record interop results at [here](https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit#gid=174659577)

* -15 plus final TLS-1.3, including omission of EOED.

* Supports all the features already present in draft-14, including connection migration.

Draft-15 brings a couple of interesting changes. It relies on a special TLS 1.3 option to remove the need to send EOED on the 0-RTT packet context before sending the Client Finished message, which does significantly simplify the implementation of the QUIC handshake. It also brings a new frame, "RETIRE CONNECTION ID", for releasing and renewing unused connection ID.

The TLS 1.3 option is materialized by a specific TLS extension, which makes the draft-15 implementation incompatible with draft-14 and lower versions. 