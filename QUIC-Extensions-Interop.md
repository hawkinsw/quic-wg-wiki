## Overview

This document lists QUIC extensions participating in the QUIC extension interop.

* QUIC Version draft-27 (vn=**ff00001b**)

* HTTP 3 (ALPN=**h3-27**)

* HTTP 0.9/1.1 (ALPN=**hq-27**)

* Record interop results [here](https://docs.google.com/spreadsheets/d/1Ygbit9UySHLMBT8FNgIIqRt2PVo1J9HLJ-yIpfjE374)

* List of implementations [here](https://github.com/quicwg/base-drafts/wiki/Implementations)

## Extensions Tested

_Feel free to add your extension to this list.  When picking a code, please do not reuse letters already defined in the [main implementation draft](16th-Implementation-Draft)._

|Extension | code | details  |
|--------------------|:---:|------------------------|
|[Delayed ACKs](https://tools.ietf.org/html/draft-iyengar-quic-delayed-ack) | a | TBD |
|[Loss bits](https://tools.ietf.org/html/draft-ferrieuxhamchaoui-quic-lossbits) | q | TBD |
|[Datagram](https://tools.ietf.org/html/draft-pauly-quic-datagram-05) | g | Can be tested using [Siduck](https://datatracker.ietf.org/doc/draft-pardue-quic-siduck/)
|[Quic-bit-grease allowed](https://https://tools.ietf.org/html/draft-thomson-quic-bit-grease-00) | b | Tested by just running against a server, and checking whether Quic Bit is greased in received packets |
|[Quic-bit-grease implemented](https://https://tools.ietf.org/html/draft-thomson-quic-bit-grease-00) | i | Tested by just running against a server, and checking whether it accepts greased Quic Bits |