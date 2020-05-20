
**This is a listing of tools for analysing, debugging and visualising QUIC (and potentially the HTTP mapping). See also the [Implementations listing](Implementations).**

# Wireshark
[Wireshark](https://wireshark.org/) has a GQUIC decoder<sup>1</sup> and IETF-QUIC decoder. ~~HTTP analysis is possible via integration with the [HTTP/2 decoder](https://wiki.wireshark.org/HTTP2).~~ http3 is not yet supported. To enable handshake/payload decryption, use a Wireshark version that matches the QUIC version:

 | # | First Wireshark version | Last WS version | notes |
 | -- | -- | -- | -- |
 | -28 | v3.3.0rc0-1246-g2b9796adc6 | | Done |
 | -27 | v3.3.0rc0-572-ge4138a3b98 / 3.2.2 | | Done |
 | -26 | v3.3.0rc0-572-ge4138a3b98 / 3.2.2 | | No wire changes. |
 | -25 | v3.3.0rc0-452-gddc03b8c87 / 3.2.2 | | Done |
 | -24 | v3.1.2rc0-16-g71e03ef042 | | Done |
 | -23 | v3.1.1rc0-323-gf95d3a6 | | Done |
 | -22 | v3.1.0rc0-1289-g3967f60 | | Done |
 | -21 | v3.1.0rc0-1288-gbafe354 | | Done |
 | -20 | v3.1.0rc0-615-g28773689e0 / 3.0.2 | 3.0.x / v3.1.0rc0-1286-gb2a437e | Done. |
 | -19 | v3.1.0rc0-520-ga65f7f5838 / 3.0.2 | 3.0.x / v3.1.0rc0-1286-gb2a437e | Done. |
 | -18 | v2.9.1rc0-487-gd486593ce3 | 3.0.x / v3.1.0rc0-1285-g954b958aa1 | Done since v2.9.1rc0-500-g064a5c90ca |
 | -17 | v2.9.1rc0-332-ga0b9e8b652 | 3.0.x / v3.1.0rc0-1285-g954b958aa1 | Done since v2.9.1rc0-456-g19630453bf |
 | -16 | v2.9.1rc0-100-g0964b04ee3 | v2.9.1rc0-331-gf1fa8df324 | Compatible with -15 (no packet change) |
 | -15 | v2.9.0rc0-2528-g9bd1c8f155 | v2.9.1rc0-331-gf1fa8df324 | Available on 2.9.0 |
 | -14 | v2.9.0rc0-1858-g0aaaa49af3 | v2.9.1rc0-108-g075785bd20 | Done. |
 | -13 | v2.9.0rc0-1850-g2fd42045f5 | v2.9.1rc0-100-g0964b04ee3 | Decryption updated. |
 | -12 | v2.9.0rc0-1816-g81710c7d3c | v2.9.0rc0-1863-g7b65208ef3 | Last draft to use TLS Exporter Secret. |
 | -11 | v2.9.0rc0-291-gee3bc52192 | v2.9.0rc0-1829-g1d2fd4f411 | +Connection migration (untested) |
 | -10 | v2.9.0rc0-200-g88435354c0 | v2.9.0rc0-1779-g351ea5940e
 | -09 | v2.5.2rc0-68-geea63ae2a7 | 2.6.x / v2.9.0rc0-173-g71ddbb69f5 | Supports payload decryption (-09) |
 | -08 | ? | v2.9.0rc0-173-g71ddbb69f5 |

Automated builds (macOS and Windows): https://www.wireshark.org/download/automated/  
Upstream bug (with sample captures/keys): https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13881  
Patches under review: https://code.wireshark.org/review/#/q/status:open+branch:master+topic:QUIC

Payload decryption (>= draft -13) requires QUIC/TLS 1.3 traffic secrets following the [SSLKEYLOGFILE key log file format](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format). OpenSSL supports this via its keylog callback.

If your QUIC traffic is not properly detected by Wireshark, note that:
- Wireshark uses a heuristics approach to detect QUIC based on the version number. If you use an experimental version number, it might not be detected as QUIC traffic.
- Dissectors using a standard port (such as 44818 for EtherNet-IP-2) usually takes precedence.
- To solve this, explicitly use *Decode As* QUIC via the GUI or via the command line option `-dudp.port==443,quic`.
- If you use non-standard draft version numbers in the version field, Wireshark will assume the latest draft version.

<sup>1</sup>Wireshark is not capable of decrypting GQUIC packets itself, even if [NSS Keylogging](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) has been configured. However, if a decrypted trace is supplied to Wireshark it will correctly dissect GQUIC if the "Force decrypt" option is enabled in the Settings.

## Wireshark draft support
<details><summary>General issues</summary>

- [x] TLS 1.3 handshake fragmentation over multiple packets. Related: https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=15537
- [x] Key Update: verify decrypted result before switching cipher. https://code.wireshark.org/review/33279
- [x] Connection migration: supported as of v2.9.0rc0-1879-g17bc055138 (tested with draft -14)
- [ ] Stream ID dissection (two LSB -> direction/initiator)
- [ ] Stateless reset (format changed again in draft -17 and -20) https://tools.ietf.org/html/draft-ietf-quic-transport-17#section-10.4
- [x] Deprecate and alias `QUIC_*SECRET*` decryption secrets for `*SECRET*` since it is the same since draft -14. https://code.wireshark.org/review/33275
- [x] Initial Follow QUIC Stream support https://code.wireshark.org/review/34694
- [ ] STREAM reassembly support
- [ ] Out-of-order and retransmission handling is not implemented and will cause decryption/dissection failures. See https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13881#c263
- [ ] Missing QPACK and HTTP/3 support. (Planned to be added.)
- [x] 0-RTT decryption support https://code.wireshark.org/review/33695
- [ ] ...
</details>

<details><summary>To-do items for draft -28 completion</summary>

- [x] New APPLICATION_ERROR (0xC) transport error code https://code.wireshark.org/review/37262
- [x] Rename TP max_packet_size (0x03) -> max_udp_payload_size. https://code.wireshark.org/review/37262
- [x] New TP for authenticating connection IDs: initial_source_connection_id (0x0f), retry_source_connection_id (0x10)
https://code.wireshark.org/review/37262 </details>

<details><summary>To-do items for draft -27 completion (completed)</summary>

- [x] Transport parameters ID and lengths now use variable-length encoding. https://code.wireshark.org/review/36150
</details>

<details><summary>To-do items for draft -25 completion (completed)</summary>

- [x] New INVALID_TOKEN, CONNECTION_ID_LIMIT_ERROR transport error https://code.wireshark.org/review/35917
- [x] New HANDSHAKE_DONE frame type https://code.wireshark.org/review/35917
- [x] Retry packet changes (Retry Integrity Tag), see commit f220d99943 https://code.wireshark.org/review/35963 https://code.wireshark.org/review/35973
- [x] Retry Integrity Tag validation https://code.wireshark.org/review/35978
- [ ] Check DecodePacketNumber implementation (#3187)
- [ ] Server have to change CID in Retry.
</details>

<details><summary>To-do items for draft -24 completion (completed)</summary>

- [x] New "quic ku" label for Key Update https://github.com/quicwg/base-drafts/commit/85db1f7181 https://code.wireshark.org/review/35145
- [ ] Forbid empty NEW_TOKEN frames.
- [ ] Client Hello can span multiple packets, need to check whether this is correctly implemented.
</details>

<details><summary>To-do items for draft -23 completion (completed)</summary>

- [x] Update initial Salt https://code.wireshark.org/review/34517 
- [x] Rename TP (disable_migration => disable_active_migration) https://code.wireshark.org/review/34517
- [x] Remove INVALID_MIGRATION error code https://code.wireshark.org/review/34517
- [X] There is now Reserved TP ( When TP = 31 * N + 27) https://code.wireshark.org/review/c/34701/
</details>

<details><summary>To-do items for draft -22 completion (completed)</summary>

- [x] Long Header/VN: DCIL(4) + SCIL(4) + DCID(0/32..144) + SCID(...) -> DCIL(8) + DCID(0..2040) + SCIL(8) + SCID(...) https://code.wireshark.org/review/33961
</details>

<details><summary>To-do items for draft -21 completion (completed)</summary>

- [x] New initial salt: 0x7fbcdb0e7c66bbe9193a96cd21519ebd7a02644a https://code.wireshark.org/review/33959
- [x] New TP: active_connection_id_limit (0x000e) https://code.wireshark.org/review/33959
- [x] CONNECTION_CLOSE/STOP_SENDING/RESET_STREAM frame: error code 16-bit -> variable length integer (max 62-bit) https://code.wireshark.org/review/33960
- [x] NEW_CONNECTION_FRAME: new Retire Prior To (i) field after Sequence Number (i) https://code.wireshark.org/review/33959
</details>

<details><summary>To-do items for draft -20 completion (completed)</summary>

- [ ] Stateless reset format has changed again. (It was not supported before anyway)
- [x] New transport error code: CRYPTO_BUFFER_EXCEEDED(0xD) https://code.wireshark.org/review/32961
</details>

<details><summary>To-do items for draft -19 completion (completed)</summary>

- [x] Removal of VERSION_NEGOTIATION_ERROR (0x9) error code.
- [x] Removal of QuicVersion fields in TransportParameters. https://code.wireshark.org/review/32833
- [x] idle_timeout (0x0001) was changed from seconds to milliseconds.
</details>

<details><summary>To-do items for draft -18 completion (completed)</summary>

- [x] Rename ACK Blocks to ACK Ranges, move First ACK Range field, rename ECN Section -> ECN Counters. https://code.wireshark.org/review/31688
- [x] Rename 0-RTT Protected -> 0-RTT https://code.wireshark.org/review/31685
- [x] Rename stream Final Offset -> Final Size; FINAL_OFFSET_ERROR -> FINAL_SIZE_ERROR https://code.wireshark.org/review/31687
- [x] PreferredAdress: split ipVersion/ipAddress in ipv4Address/ipv4Port/ipv6Address/ipv6Port fields. https://code.wireshark.org/review/31689
</details>

<details><summary>To-do items for draft -17 completion (completed)</summary>

- [x] Update PNE -> Header protection, update initial salt, update HKDF label. https://code.wireshark.org/review/31480
- [x] Packet number decryption fixes. https://code.wireshark.org/review/31634
- [x] Display unprotected short header bytes, fix 1RTT decryption (incl. KeyUpdate?, untested) https://code.wireshark.org/review/31637
- [x] Renumbered frames (and rename like BLOCKED -> DATA_BLOCKED, STREAM_BLOCKED -> STREAM_DATA_BLOCKED). https://code.wireshark.org/review/31405
- [x] Renumbered transport parameters (TP) and use varints, rename `initial_max_bidi_streams` -> `initial_max_streams_bidi` (likewise for `uni`). https://code.wireshark.org/review/31534
- [x] NEW_CONNECTION_ID: move Sequence(i) field before CID Length field... (revert draft-15 change!). https://code.wireshark.org/review/31405
- [x] Add Spin bit (short header) https://code.wireshark.org/review/31644
- [x] Display unprotected long header bytes. https://code.wireshark.org/review/31642
</details>

<details><summary>To-do items for draft -16 completion (completed)</summary>

- [x] Add draft-16 to quic_versions_vals https://code.wireshark.org/review/31169
</details>

<details><summary>To-do items for draft -15 completion (completed)</summary>

- [X] Merge ACK and ACK\_ECN. Renumbers ACK(0x0d) -> ACK(0x1b). (ECN is like ACK frame, but with ECN Section after it) https://code.wireshark.org/review/30420 https://code.wireshark.org/review/30491
- [X] Add 2 transport parameters: max\_ack\_delay(12) and original\_connection\_id(13) https://code.wireshark.org/review/30418
- [X] NEW_CONNECTION_ID: move Sequence(i) field after CID Length field. https://code.wireshark.org/review/30419
- [X] Add RETIRE\_CONNECTION\_ID(0x0d) type (NOTE: conflict with old ACK(0x0d)). https://code.wireshark.org/review/30492
</details>

<details><summary>To-do items for draft -14 completion (completed)</summary>

- [x] Retry Packet: completely changed. https://code.wireshark.org/review/29689
- [x] ACK\_ECN Change value (0x20) => (0x1a)  https://code.wireshark.org/review/29702
- [x] Remove error code: UNSOLICITED\_PATH\_RESPONSE https://code.wireshark.org/review/29703 
- [x] Split initial\_max\_stream\_data (0) into initial\_max\_stream\_data\_bidi\_local (0), initial\_max\_stream\_data\_bidi\_remote (10), initial\_max\_stream\_data\_uni (11) https://code.wireshark.org/review/29722
</details>

<details><summary>To-do items for draft -13 completion (more or less complete)</summary>

- [x] Long header: "Payload Length" -> "Length" (length of following PKN + payload)
- [x] Initial Packet: can now be sent by server as well, contains Token Length + Token fields following the normal long header. https://code.wireshark.org/review/29641
- [x] New transport parameter: disable\_migration (9) https://code.wireshark.org/review/29674 
- [ ] Stateless Reset packet format change (due to short header type changes)
- [x] CONNECTION\_CLOSE: gains new Frame Type (i) field. https://code.wireshark.org/review/29698 
- [x] New frame type: CRYPTO (0x18). Replaces "Stream 0" and changes how Initial Packet/Handshake are used.
  - [x] Recognize CRYPTO frame. https://code.wireshark.org/review/29642
  - [x] Process TLS handshake/alert messages using QUIC as framing and protection layer. https://code.wireshark.org/review/29677
- [x] Retry Packet: no longer carries a TLS HRR, see [4.4.2](https://tools.ietf.org/html/draft-ietf-quic-transport-13#section-4.4.2). https://code.wireshark.org/review/29687
- [x] New frame type: NEW\_TOKEN (0x19) https://code.wireshark.org/review/29699 
- [x] New frame type: ACK\_ECN (0x20) https://code.wireshark.org/review/29699 
- [x] New QUIC Frame Type Registry with IANA. Verified matching.
- [x] Renamed error: FRAME_FORMAT_ERROR -> FRAME_ENCODING_ERROR (0x7) https://code.wireshark.org/review/29700
- [x] New error type: INVALID_MIGRATION (0xC) https://code.wireshark.org/review/29700
- [ ] Changed error definition: FRAME_ERROR -> CRYPTO_ERROR (0x1XX) https://code.wireshark.org/review/29740
- [x] TLS extension number change: quic_transport_parameter(26) -> 0xffa5 https://code.wireshark.org/review/29673 
</details>

<details><summary>To-do items for draft -12 completion (completed and obsolete)</summary>

- [x] Short packet: two type bits -> reserved. https://code.wireshark.org/review/29668
- [x] Packet number encryption (starts at zero, there is no special Initial Packet Number). Replaces previous "packet number gap" approach. https://code.wireshark.org/review/29637
- [x] 7, 14, 30-bit variable length packet numbers https://code.wireshark.org/review/29637
- [x] New transport parameter: preferred\_address (4) https://code.wireshark.org/review/29671
- [ ] Improve connection migration tracking: use Server's Preferred Address
</details>

<details><summary>To-do items for draft -11 completion (completed and obsolete)</summary>

- [x] new short header flags, long header format https://code.wireshark.org/review/27009
- [x] packet coalescing. Draft -12 clarifies: applies to short packet headers too; packets (within a datagram) with different DCID than the first packet should be ignored. https://code.wireshark.org/review/29607 (framing only, decryption of multiple messages is incomplete)
- [x] storing CID for reference in short header packet https://code.wireshark.org/review/27098
- [x] update NEW_CONNECTION_ID dissection https://code.wireshark.org/review/27107
- [ ] connection tracking based on CID / connection migration
  - [x] Basic connection tracking https://code.wireshark.org/review/27068
  - [ ] Use NEW_CONNECTION_ID hint (requires user to provide EXPORTER_SECRET keys)
</details>


# QUIC Tracker
[QUIC-Tracker](https://quic-tracker.info.ucl.ac.be/) is a test suite for IETF-QUIC. It exchanges packets with IETF-QUIC implementations to verify whether an implementation conforms with the IETF specification. The test suite is consisting of several test scenarii. Each of them tests a particular feature of the QUIC protocol. The test suite runs daily, and its results are available on its website.

It currently supports QUIC draft-22 and TLS 1.3.

# qvalve

[qvalve](https://github.com/NTAP/qvalve) can predictably impair QUIC flows, by dropping, reordering or duplicating individual packets and sequences of packets. It is a non-transparent UDP proxy that should be interposed between a QUIC client and a QUIC server.
The behavior of qvalve is configured with rules specified in a simple language. 

# [spindump](https://github.com/EricssonResearch/spindump)
The "Spindump" tool is a Unix command-line utility that can be used for latency monitoring in traffic passing through an interface. The tool performs passive, in-network monitoring. It is not a tool to monitor traffic content or metadata of individual connections, and indeed that is not possible in the Internet as most connections are encrypted. The tool looks at the characteristics of transport protocols, such as the QUIC Spin Bit, and attempts to derive information about round-trip times for individual connections or for the aggregate or average values. The tool supports TCP, QUIC, COAP, DNS, and ICMP traffic, as well as both IPv4 and IPv6.

- **Language:** C
- **Version:** google QUIC, draft-16, draft-17, draft-18, draft-19, draft-20, draft-21, draft-22, draft-23, draft-24, draft-25
- **Roles:** in-network tool
- **Handshake:** QUIC only, does not peek into TLS or HTTP messaging inside
- **Protocol IDs:** `0x00000001` `0xff000010`, `0xff000011`, `0xff000012`, `0xff000013`, `0xff000014`, `0xff000015`, `0xff000016`, `0xff000017`, `0xff000018`,`0xff000019`,`0x50435131`, etc.
- **Public server:** n.a.

# [h2load](https://github.com/nghttp2/nghttp2/tree/quic)

h2load is load testing tool and now experimentally supports HTTP/3.

- **Language:** C++
- **Version:** draft-27
- **Roles:** client
- **Handshake:** TLS 1.3
- **Protocol IDs:** `0xff00001b`
- **ALPN:** `h3-27`

# [qlog](https://github.com/quiclog/internet-drafts)

qlog is a standard logging format for QUIC and HTTP/3. It is based on JSON to be both human and machine-readable. It is currently supported by over half of current QUIC implementations.

- Main schema: https://quiclog.github.io/internet-drafts/draft-marx-qlog-main-schema.html
- Events definitions for QUIC and HTTP/3: https://quiclog.github.io/internet-drafts/draft-marx-qlog-event-definitions-quic-h3.html
- TypeScript definitions: https://github.com/quiclog/qlog/tree/master/TypeScript/draft-01
- Rust crate: https://crates.io/crates/qlog
- Tool to transform decrypted pcap to qlog: https://github.com/quiclog/pcap2qlog

# [qvis](https://qvis.edm.uhasselt.be)

Toolsuite for visualizing QUIC+HTTP/3 qlog and pcap files. Includes a sequence diagram, congestion graph, multiplexing diagram and packetization visualization. 

- Live version (includes demo files): https://qvis.edm.uhasselt.be/
- Source code: https://github.com/quiclog/qvis

# [QUIC network simulator](https://github.com/marten-seemann/quic-network-simulator/)

Simulation framework for automated benchmarking of QUIC implementations. Allows extensive network simulations using ns-3. 

Source code: https://github.com/marten-seemann/quic-network-simulator/

# [QUIC interop runner](https://interop.seemann.io/)

Interoperability testing framework. Allows running automated tests between different QUIC implementations. 

- Live version: https://interop.seemann.io/
- Source code: https://github.com/marten-seemann/quic-interop-runner



