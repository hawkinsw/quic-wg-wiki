# QUIC-Related IETF Activities and Proposals 

This page collects information about the various QUIC-related proposals and activities happening or proposed for the wider IETF space. Please add to this list and update the various items as needed. Feel free to use the given template:

```
### Short Activity/Proposal Title
A sentence or three on what this is about and how it relates to the chartered QUIC work in the IETF.
* **Main contact:** Names and email addresses of a small number of main contacts.
* **Forum:** Mailing list, Slack channel, etc. for interested parties to learn more and join the effort.
* **Materials:** GitHub repo, I-Ds, papers, etc. Links to relevant material.
```

## Active Proposals

Below is a (not necessarily complete) list of things the chairs are aware of at the moment. Whoever feels in charge for one of the items below should feel free to create an entry (using the template) in this section, and then remove the bullet item from the list:

### QUIC-LB

Servers generate their Connection IDs, but numerous trusted devices it the path (load balancers, DDOS boxes, crypto offload, local switch disaggregators) need access to some of the encoded data in those CIDs, while that same data is opaque to untrusted observers. QUIC-LB defines multiple encoding strategies to allow this cooperation between servers and trusted intermediaries.

* **Main contact:** Martin Duke, martin.h.duke@gmail.com
* **Forum:** There is a google group. Email Martin to join.
* **Materials:** [[https://github.com/martinduke/draft-duke-quic-load-balancers]], https://datatracker.ietf.org/doc/draft-duke-quic-load-balancers/

### Datagram Payloads

Some applications, particularly those that need to transmit real-time data, prefer to transmit data unreliably. These applications can build directly upon UDP as a transport, and can add security with DTLS. Extending QUIC to support transmitting unreliable application data would provide another option for secure datagrams, with the added benefit of sharing a cryptographic and authentication context used for reliable streams.

This proposal defines DATAGRAM QUIC frame types, which carry application data without requiring retransmissions.

* **Main contact:** Tommy Pauly, tpauly@apple.com
* **Forum:** datagram channel in QUIC slack, or QUIC email list.
* **Materials:** https://github.com/tfpauly/draft-pauly-quic-datagram

### qlog logging format and associated tooling and visualizations
qlog is a proposal for a flexible, interoperable endpoint logging format/schema spanning multiple modern protocols and networking use cases, as well as its associated tooling. The idea is that all QUIC and HTTP/3 implementations would output logs in the same JSON-based format, making it easier to write reusable (browser-based) tools. The format is evolving rapidly and we welcome feedback and implementation experience. Currently supported (partially) by quicker, lsquic, mvfst, quant and aioquic. 

* **Main contact:** Robin Marx, robin.marx@uhasselt.be
* **Forum:** qlog@ietf.org, #qlog in the QUIC slack, github issues on the repo (see below)
* **Materials:** https://github.com/quiclog, https://tools.ietf.org/html/draft-marx-qlog-main-schema-00, https://tools.ietf.org/html/draft-marx-qlog-event-definitions-quic-h3-00, https://quic.edm.uhasselt.be, https://quic.edm.uhasselt.be/qtr-to-qlog/

### Loss Bits

Packet loss detection and localization is critical to operating networks.  Using two loss bits, preferably in QUIC header, allows passive observers to identify, measure, and localize loss as upstream or downstream of the observer.  The proposal is supported by a real world implementation with data derived from actual end-user connections in a number of countries.

* **Main contacts:** Lubashev, Igor <ilubashe@akamai.com>; Alexandre Ferrieux <alexandre.ferrieux@orange.com>; Isabelle Hamchaoui <isabelle.hamchaoui@orange.com>
* **Forum:** github issues on the repo (see below); TBD for mailing list (ETA: Wednesday)
* **Materials:**
** GitHub: https://github.com/igorlord/draft-ferrieuxhamchaoui-tsvwg-lossbits
** I-D: https://tools.ietf.org/html/draft-ferrieuxhamchaoui-tsvwg-lossbits
** Slides: TBD (ETA: Friday)

### Other proposals

* MASQUE
* media
* multipath
* auth handshake
* 0rtt-bdp
* multicast 
* satellite / etosat
* FEC
* quic-network-sim


## Historic Proposals
