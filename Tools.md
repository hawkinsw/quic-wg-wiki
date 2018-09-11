
**This is a listing of tools for analysing, debugging and visualising QUIC (and potentially the HTTP mapping). See also the [Implementations listing](Implementations).**

# Wireshark
[Wireshark](https://wireshark.org/) has a GQUIC decoder<sup>1</sup> and IETF-QUIC decoder. HTTP analysis is possible via integration with the [HTTP/2 decoder](https://wiki.wireshark.org/HTTP2). To enable handshake decryption, use a Wireshark version that matches the QUIC version:

 | draft | First Wireshark version | Last WS version | notes |
 | -- | -- | -- | -- |
 | -13 | | | TODO |
 | -12 | | | WIP |
 | -11 | v2.9.0rc0-291-gee3bc52192 | | +Connection migration (untested), WIP |
 | -10 | v2.9.0rc0-200-g88435354c0 | v2.9.0rc0-1779-g351ea5940e
 | -09 | v2.5.2rc0-68-geea63ae2a7 | 2.6.x / v2.9.0rc0-173-g71ddbb69f5 | Supports payload decryption (-09) |
 | -08 | ? | v2.9.0rc0-173-g71ddbb69f5 |

To enable payload decryption, the TLS Exporter secret is required which must be provided via a TLS key log file. See for example https://github.com/ngtcp2/ngtcp2/pull/67. Note that since OpenSSL_1_1_1-pre5-21-gd4da95a773 (2018-04-18), OpenSSL supports this via its keylog callback.

Automated builds (macOS and Windows) for (odd-numbered) development versions: https://www.wireshark.org/download/automated/  
Upstream bug: https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13881  
Patches under review: https://code.wireshark.org/review/#/q/status:open+branch:master+topic:QUIC

To-do items for draft -13 completion:
- [ ] Long header: "Payload Length" -> "Length" (length of following PKN + payload)
- [ ] Initial Packet: can now be sent by server as well, contains Token Length + Token fields following the normal long header.
- [ ] New transport parameter: disable\_migration (9)
- [ ] Stateless Reset packet format change (due to short header type changes)
- [ ] CONNECTION\_CLOSE: gains new Frame Type (i) field.
- [ ] New frame type: CRYPTO (0x18). Replaces "Stream 0" and changes how Initial Packet/Handshake are used.
- [ ] New frame type: NEW\_TOKEN (0x19)
- [ ] New frame type: ACK\_ECN (0x20)
- [ ] New QUIC Frame Type Registry with IANA
- [ ] Renamed error: FRAME_FORMAT_ERROR -> FRAME_ENCODING_ERROR (0x7)
- [ ] New error type: INVALID_MIGRATION (0xC)
- [ ] Changed error definition: FRAME_ERROR -> CRYPTO_ERROR (0x1XX)
- [ ] TLS extension number change: quic_transport_parameter(26) -> 0xffa5

To-do items for draft -12 completion:
- [ ] Short packet: two type bits -> reserved
- [ ] Packet number encryption (starts at zero, there is no special Initial Packet Number). Replaces previous "packet number gap" approach.
- [ ] 7, 14, 30-bit variable length packet numbers
- [ ] Retry Packet - Packet Number MUST be 0. (but subject to change?)
- [ ] New transport parameter: preferred\_address (4)
- [ ] Server's Preferred Address (connection migration related)
- [ ] STREAM frames can now be empty.

To-do items for draft -11 completion:
- [x] new short header flags, long header format https://code.wireshark.org/review/27009
- [x] packet coalescing. Draft -12 clarifies: applies to short packet headers too; packets (within a datagram) with different DCID than the first packet should be ignored. https://code.wireshark.org/review/29607
- [x] storing CID for reference in short header packet https://code.wireshark.org/review/27098
- [x] update NEW_CONNECTION_ID dissection https://code.wireshark.org/review/27107
- [ ] connection tracking based on CID / connection migration
  - [x] Basic connection tracking https://code.wireshark.org/review/27068
  - [ ] Use NEW_CONNECTION_ID hint (requires user to provide EXPORTER_SECRET keys)
  - [ ] Testing with actual implementation

<sup>1</sup> Wireshark is not capable of decrypting GQUIC packets itself, even if [NSS Keylogging](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) has been configured. However, if a decrypted trace is supplied to Wireshark it will correctly dissect GQUIC if the "Force decrypt" option is enabled in the Settings.

# QUIC Tracker
[QUIC-Tracker](https://quic-tracker.info.ucl.ac.be/) is a test suite for IETF-QUIC. It exchanges packets with IETF-QUIC implementations to verify whether an implementation conforms with the IETF specification. The test suite is consisting of several test scenarii. Each of them aims at testing a particular feature of the QUIC protocol. The test suite runs daily, and its results are available on its website.

It currently supports QUIC draft-14 and TLS 1.3.