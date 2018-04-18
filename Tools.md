
**This is a listing of tools for analysing, debugging and visualising QUIC (and potentially the HTTP mapping). See also the [Implementations listing](Implementations).**

# Wireshark
[Wireshark](https://wireshark.org/) has a GQUIC decoder<sup>1</sup> and IETF-QUIC decoder. HTTP analysis is possible via integration with the [HTTP/2 decoder](https://wiki.wireshark.org/HTTP2). To enable handshake decryption, use a Wireshark version that matches the QUIC version:

 | draft | First Wireshark version | Last WS version | notes |
 | -- | -- | -- | -- |
 | -11 | | | WIP |
 | -10 | v2.9.0rc0-200-g88435354c0 | git master |
 | -09 | v2.5.2rc0-68-geea63ae2a7 | 2.6.x / v2.9.0rc0-173-g71ddbb69f5 | Supports payload decryption (-09) |
 | -08 | ? | v2.9.0rc0-173-g71ddbb69f5 |

To enable payload decryption, the TLS Exporter secret is required which must be provided via a TLS key log file. See for example https://github.com/ngtcp2/ngtcp2/pull/67. Note that since OpenSSL_1_1_1-pre5-21-gd4da95a773 (2018-04-18), OpenSSL supports this via its keylog callback.

Automated builds (macOS and Windows) for (odd-numbered) development versions: https://www.wireshark.org/download/automated/
Upstream bug: https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13881

To-do items for draft -11 completion:
- [x] new short header flags, long header format https://code.wireshark.org/review/27009
- [ ] packet coalescing
- [ ] storing CID for reference in short header packet
- [ ] connection tracking based on CID / connection migration
- [ ] update NEW_CONNECTION_ID dissection

<sup>1</sup> Wireshark is not capable of decrypting GQUIC packets itself, even if [NSS Keylogging](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) has been configured. However, if a decrypted trace is supplied to Wireshark it will correctly dissect GQUIC if the "Force decrypt" option is enabled in the Settings. For IETF-QUIC, It is expected that the move to TLS1.3 will allow decryption.

# QUIC Tracker
[QUIC-Tracker](https://quic-tracker.info.ucl.ac.be/) is a test suite for IETF-QUIC. It exchanges packets with IETF-QUIC implementations to verify whether an implementation conforms with the IETF specification. The test suite is consisting of several test scenarii. Each of them aims at testing a particular feature of the QUIC protocol. The test suite runs daily, and its results are available on its website.