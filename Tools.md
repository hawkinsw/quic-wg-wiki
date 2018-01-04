
**This is a listing of tools for analysing, debugging and visualising QUIC (and potentially the HTTP mapping). See also the [Implementations listing](Implementations).**

* [Wireshark](https://wireshark.org/) has a GQUIC decoder<sup>1</sup> and IETF-QUIC decoder (draft05 to draft08). HTTP analysis is possible via integration with the [HTTP/2 decoder](https://wiki.wireshark.org/HTTP2).

---

<sup>1</sup> Wireshark is not capable of decrypting GQUIC packets itself, even if [NSS Keylogging](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format) has been configured. However, if a decrypted trace is supplied to Wireshark it will correctly dissect GQUIC if the "Force decrypt" option is enabled in the Settings. For IETF-QUIC, It is expected that the move to TLS1.3 will allow decryption.