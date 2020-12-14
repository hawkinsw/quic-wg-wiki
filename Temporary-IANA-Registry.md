QUIC implementers have started experimenting with QUIC extensions, and have a need for codepoints to do interop testing. To avoid collisions, please list your experimental codepoints below. This page will be deleted once the official IANA registry is operational.

## QUIC Transport Parameters

| Value  | Parameter Name              | Specification                       |
|:-------|:----------------------------|:------------------------------------|
|  0x00  | original_destination_connection_id      | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x01  | max_idle_timeout            | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x02  | stateless_reset_token       | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x03  | max_udp_payload_size             | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x04  | initial_max_data            | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x05  | initial_max_stream_data_bidi_local | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x06  | initial_max_stream_data_bidi_remote | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x07  | initial_max_stream_data_uni | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x08  | initial_max_streams_bidi    | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x09  | initial_max_streams_uni     | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0a  | ack_delay_exponent          | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0b  | max_ack_delay               | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0c  | disable_active_migration    | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0d  | preferred_address           | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0e  | active_connection_id_limit  | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x0f  | initial_connection_id       | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x10  | retry_connection_id         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-parameter-re) |
|  0x1B  | reserved for GREASE         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-reserved-transport-paramete) |
|  0x20  | max_datagram_frame_size     | [quic-datagram](https://tools.ietf.org/html/draft-pauly-quic-datagram-05#section-7) |
|  0x3A  | reserved for GREASE         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-reserved-transport-paramete) |
|  0x40  | max_sending_uniflow_id      | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
|  0x41  | enable_multipath            | [multipath-quic](https://datatracker.ietf.org/doc/draft-liu-multipath-quic/) |
|  0x59  | reserved for GREASE         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-reserved-transport-paramete) |
| 0x1057 | loss_bits                   | [quic-lossbits](https://tools.ietf.org/html/draft-ferrieuxhamchaoui-quic-lossbits) |
| 0x2ab2 | grease_quic_bit             | [quic-bit-grease](https://tools.ietf.org/html/draft-thomson-quic-bit-grease) |
| 0x173E | discard                   | [Discard upon receipt](https://github.com/quicwg/base-drafts/wiki/Quantum-Readiness-test) |
| 0x3127 | Google_internal | Google-internal undocumented transport parameter |
| 0x3128 | Google_internal | Google-internal undocumented transport parameter |
| 0x3129 | Google_internal | Google-internal undocumented transport parameter |
| 0x312A | Google_internal | Google-internal undocumented transport parameter |
| 0x312B | Google_internal | Google-internal undocumented transport parameter |
| 0x312C | Google_internal | Google-internal undocumented transport parameter |
| 0x312D | Google_internal | Google-internal undocumented transport parameter |
| 0x312E | Google_internal | Google-internal undocumented transport parameter |
| 0x312F | Google_internal | Google-internal undocumented transport parameter |
| 0x3130 | Google_internal | Google-internal undocumented transport parameter |
| 0x3131 | Google_internal | Google-internal undocumented transport parameter |
| 0x3132 | Google_internal | Google-internal undocumented transport parameter |
| 0x3133 | Google_internal | Google-internal undocumented transport parameter |
| 0x3134 | Google_internal | Google-internal undocumented transport parameter |
| 0x465A | Google_internal | Google-internal undocumented transport parameter |
| 0x4751 | Google_internal | Google-internal undocumented transport parameter |
| 0x4752 | Google_internal | Google-internal undocumented transport parameter |
| 0x7157 | enable_time_stamp (v0) | [quic-ts](https://datatracker.ietf.org/doc/draft-huitema-quic-ts/) |
| 0x7158 | enable_time_stamp (v1) | [quic-ts](https://datatracker.ietf.org/doc/draft-huitema-quic-ts/) |
| 0x73DB | version_negotiation         | [quic-version-negotiation](https://tools.ietf.org/html/draft-schinazi-quic-version-negotiation-02#section-9) |
| 0xBAAD | disable_1rtt_encryption     | [quic-disable-encryption](https://tools.ietf.org/html/draft-banks-quic-disable-encryption) |
| 0xDE1A | min_ack_delay               | [quic-delayed-ack-00](https://tools.ietf.org/html/draft-iyengar-quic-delayed-ack-00) |
| 0xFF02DE1A | min_ack_delay           | [quic-delayed-ack-02](https://tools.ietf.org/html/draft-iyengar-quic-delayed-ack-02) |


## QUIC Frame Types

| Type Value  | Frame Type Name      | Specification                  |
|:------------|:---------------------|:-------------------------------|
| 0x00        | PADDING              | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x01        | PING                 | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x02 - 0x03 | ACK                  | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x04        | RESET_STREAM         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x05        | STOP_SENDING         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x06        | CRYPTO               | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x07        | NEW_TOKEN            | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x08 - 0x0f | STREAM               | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x10        | MAX_DATA             | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x11        | MAX_STREAM_DATA      | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x12 - 0x13 | MAX_STREAMS          | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x14        | DATA_BLOCKED         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x15        | STREAM_DATA_BLOCKED  | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x16 - 0x17 | STREAMS_BLOCKED      | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x18        | NEW_CONNECTION_ID    | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x19        | RETIRE_CONNECTION_ID | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x1a        | PATH_CHALLENGE       | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x1b        | PATH_RESPONSE        | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x1c - 0x1d | CONNECTION_CLOSE     | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x1e        | HANDSHAKE_DONE       | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#frame-types) |
| 0x22 - 0x23 | ACK_MP               | [multipath-quic](https://datatracker.ietf.org/doc/draft-liu-multipath-quic/) |
| 0x24        | QOE_CONTROL_SIGNALS  | [multipath-quic](https://datatracker.ietf.org/doc/draft-liu-multipath-quic/) |
| 0x2a        | PATH_STATUS          | [multipath-quic](https://datatracker.ietf.org/doc/draft-liu-multipath-quic/) |
| 0x30 - 0x31 | DATAGRAM | [quic-datagram](https://tools.ietf.org/html/draft-pauly-quic-datagram-05#section-7) |
| 0x40        | MP_NEW_CONNECTION_ID    | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0x41        | MP_RETIRE_CONNECTION_ID | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0x42 - 0x43 | MP_ACK                  | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0x44        | ADD_ADDRESS             | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0x45        | REMOVE_ADDRESS          | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0x46        | UNIFLOWS                | [quic-multipath](https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/) |
| 0xAF        | ACK_FREQUENCY        | [quic-delayed-ack](https://tools.ietf.org/html/draft-iyengar-quic-delayed-ack) |
| 0x02F5 | TIME_STAMP | [quic-ts](https://datatracker.ietf.org/doc/draft-huitema-quic-ts/) |

## QUIC Transport Error Codes

| Value | Error                     | Description                   | Specification   |
|:------|:--------------------------|:------------------------------|:----------------|
| 0x0   | NO_ERROR                  | No error                      | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x1   | INTERNAL_ERROR            | Implementation error          | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x2   | SERVER_BUSY               | Server currently busy         | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x3   | FLOW_CONTROL_ERROR        | Flow control error            | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x4   | STREAM_LIMIT_ERROR        | Too many streams opened       | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x5   | STREAM_STATE_ERROR        | Frame received in invalid stream state | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x6   | FINAL_SIZE_ERROR          | Change to final size          | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x7   | FRAME_ENCODING_ERROR      | Frame encoding error          | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x8   | TRANSPORT_PARAMETER_ERROR | Error in transport parameters | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x9   | CONNECTION_ID_LIMIT_ERROR | Too many connection IDs received | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xA   | PROTOCOL_VIOLATION        | Generic protocol violation    | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xB   | INVALID_TOKEN             | Invalid Token Received        | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xC   | APPLICATION_ERROR         | Application error           | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xD   | CRYPTO_BUFFER_EXCEEDED    | CRYPTO data buffer overflowed | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xE   | KEY_UPDATE_ERROR          | Invalid packet protection update | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0xF   | AEAD_LIMIT_REACHED        | Excessive use of packet protection keys | [quic-transport](https://quicwg.org/base-drafts/draft-ietf-quic-transport.html#name-quic-transport-error-codes-) |
| 0x100-0x1ff   | CRYPTO_ERROR    | TLS error | [quic-tls](https://quicwg.org/base-drafts/draft-ietf-quic-tls.html#name-tls-errors) |
| 0x53F8 | VERSION_NEGOTIATION_ERROR | Version Negotiation Error | [quic-version-negotiation](https://tools.ietf.org/html/draft-schinazi-quic-version-negotiation-02#section-9) |

## HTTP/3 Frame Types

| Frame Type       | Value  | Specification            |
|------------------|--------|--------------------------|
| DATA             |  0x0   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| HEADERS          |  0x1   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| Reserved         |  0x2   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| CANCEL_PUSH      |  0x3   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| SETTINGS         |  0x4   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| PUSH_PROMISE     |  0x5   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| Reserved         |  0x6   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| GOAWAY           |  0x7   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| Reserved         |  0x8   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| Reserved         |  0x9   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| MAX_PUSH_ID      |  0xD   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| DUPLICATE_PUSH   |  0xE   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-frame-types) |
| PRIORITY_UPDATE  |  0xF   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |
| PRIORITY_UPDATE  |  0x10   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |
| PRIORITY_UPDATE  |  0xF0700   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |
| PRIORITY_UPDATE  |  0xF0701   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |
| PRIORITY_UPDATE  |  0x1CCB8BBF1F0700   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |
| PRIORITY_UPDATE  |  0x1CCB8BBF1F0701   | [http-priorities](https://httpwg.org/http-extensions/draft-ietf-httpbis-priority.html#section-5.2) |

# HTTP/3 Settings Parameters

| Setting Name                 |  Value | Specification             |
| ---------------------------- | :----: | ------------------------- |
| QPACK_MAX_TABLE_CAPACITY     | 0x1    | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-settings-registration)
| Reserved                     |  0x2   | N/A                       |
| Reserved                     |  0x3   | N/A                       |
| Reserved                     |  0x4   | N/A                       |
| Reserved                     |  0x5   | N/A                       |
| MAX_HEADER_LIST_SIZE         |  0x6   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-settings-parameters) |
| QPACK_BLOCKED_STREAMS        | 0x7    | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-settings-registration) |
| H3_DATAGRAM                  | 0x276  | [h3-datagram](https://tools.ietf.org/html/draft-schinazi-quic-h3-datagram) |
| ENABLE_WEBTRANSPORT          | 0x2b603742 | [h3-webtransport](https://tools.ietf.org/html/draft-vvv-webtransport-http3) |


## HTTP/3 Error Codes

| Name                              | Value      | Description                              | Specification          |
| --------------------------------- | ---------- | ---------------------------------------- | ---------------------- |
| H3_NO_ERROR                       | 0x0100     | No error                                 | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_GENERAL_PROTOCOL_ERROR         | 0x0101     | General protocol error                   | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_INTERNAL_ERROR                 | 0x0102     | Internal error                           | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_STREAM_CREATION_ERROR          | 0x0103     | Stream creation error                    | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_CLOSED_CRITICAL_STREAM         | 0x0104     | Critical stream was closed               | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_FRAME_UNEXPECTED               | 0x0105     | Frame not permitted in the current state | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_FRAME_ERROR                    | 0x0106     | Frame violated layout or size rules      | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_EXCESSIVE_LOAD                 | 0x0107     | Peer generating excessive load           | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_ID_ERROR                       | 0x0108     | An identifier was used incorrectly       | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_SETTINGS_ERROR                 | 0x0109     | SETTINGS frame contained invalid values  | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_MISSING_SETTINGS               | 0x010A     | No SETTINGS frame received               | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_REQUEST_REJECTED               | 0x010B     | Request not processed                    | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_REQUEST_CANCELLED              | 0x010C     | Data no longer needed                    | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_REQUEST_INCOMPLETE             | 0x010D     | Stream terminated early                  | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_CONNECT_ERROR                  | 0x010F     | TCP reset or error on CONNECT request    | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| H3_VERSION_FALLBACK               | 0x0110     | Retry over HTTP/1.1                      | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-error-codes)   |
| QPACK_DECOMPRESSION_FAILED   | 0x200 | Decompression of a header block failed   | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-error-code-registration) |
| QPACK_ENCODER_STREAM_ERROR   | 0x201 | Error on the encoder stream              | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-error-code-registration) |
| QPACK_DECODER_STREAM_ERROR   | 0x202 | Error on the decoder stream              | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-error-code-registration) |

## HTTP/3 Stream Types

| Stream Type      | Value  | Specification              |
| ---------------- | :----: | -------------------------- |
| Control Stream   |  0x00  | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-stream-types) |
| Push Stream      |  0x01  | [quic-http](https://quicwg.org/base-drafts/draft-ietf-quic-http.html#name-stream-types) |
| QPACK Encoder Stream         | 0x02   | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-stream-type-registration) |
| QPACK Decoder Stream         | 0x03   | [quic-qpack](https://quicwg.org/base-drafts/draft-ietf-quic-qpack.html#name-stream-type-registration) |



