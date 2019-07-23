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
* **Forum:** There is a Google group. Email Martin to join.
* **Materials:**
  * https://github.com/martinduke/draft-duke-quic-load-balancers
  * https://datatracker.ietf.org/doc/draft-duke-quic-load-balancers/

### Datagram Payloads

Some applications, particularly those that need to transmit real-time data, prefer to transmit data unreliably. These applications can build directly upon UDP as a transport, and can add security with DTLS. Extending QUIC to support transmitting unreliable application data would provide another option for secure datagrams, with the added benefit of sharing a cryptographic and authentication context used for reliable streams.

This proposal defines DATAGRAM QUIC frame types, which carry application data without requiring retransmissions.

* **Main contact:** Tommy Pauly, tpauly@apple.com
* **Forum:** #datagram channel in QUIC slack, or QUIC email list.
* **Materials:** https://github.com/tfpauly/draft-pauly-quic-datagram

### qlog logging format and associated tooling and visualizations
qlog is a proposal for a flexible, interoperable endpoint logging format/schema spanning multiple modern protocols and networking use cases, as well as its associated tooling. The idea is that all QUIC and HTTP/3 implementations would output logs in the same JSON-based format, making it easier to write reusable (browser-based) tools. The format is evolving rapidly and we welcome feedback and implementation experience. Currently supported (partially) by quicker, lsquic, mvfst, quant and aioquic. 

* **Main contact:** Robin Marx, robin.marx@uhasselt.be
* **Forum:** qlog@ietf.org, #qlog in the QUIC slack, github issues on the repo (see below)
* **Materials:**
  * https://github.com/quiclog
  * https://tools.ietf.org/html/draft-marx-qlog-main-schema
  * https://tools.ietf.org/html/draft-marx-qlog-event-definitions-quic-h3
  * https://quic.edm.uhasselt.be
  * https://quic.edm.uhasselt.be/qtr-to-qlog/

### Loss Bits

Packet loss detection and localization is critical to operating networks.  Using two loss bits, preferably in QUIC header, allows passive observers to identify, measure, and localize loss as upstream or downstream of the observer.  The proposal is supported by a real world implementation with data derived from actual end-user connections in a number of countries.

* **Main contacts:** 
  * Lubashev, Igor <ilubashe@akamai.com>
  * Alexandre Ferrieux <alexandre.ferrieux@orange.com>
  * Isabelle Hamchaoui <isabelle.hamchaoui@orange.com>
* **Forum:**
  * github issues on the repo (see below)
  * TBD for mailing list (ETA: Wednesday)
* **Materials:**
  * GitHub: https://github.com/igorlord/draft-ferrieuxhamchaoui-tsvwg-lossbits
  * I-D: https://tools.ietf.org/html/draft-ferrieuxhamchaoui-tsvwg-lossbits
  * Slides: TBD (ETA: Friday)

### 0rtt-bdp

This memo discusses a solution where the client is informed about path parameters: both the client and the server can contribute to the reduction of the time-to-service for subsequent connections.
* **Main contact:**
  * Nicolas Kuhn (nicolas.kuhn@cnes.fr)
  * Emile Stephan (emile.stephan@orange.com)
  * Gorry Fairhurst (gorry@erg.abdn.ac.uk)
* **Forum:** QUIC mailing list or GitHub issues on the repo
* **Materials:** 
  * GitHub: https://github.com/NicoKos/QUIC_HIGH_BDP
  * I-D: https://datatracker.ietf.org/doc/draft-kuhn-quic-0rtt-bdp/ 

### satellite / etosat
This memo identifies the characteristics of a SATCOM link that impact the operation of the QUIC transport protocol and proposes best current practice to ensure acceptable protocol performance.
* **Main contact:** 
  * Nicolas Kuhn (nicolas.kuhn@cnes.fr)
  * Gorry Fairhurst (gorry@erg.abdn.ac.uk)
  * John Border (border@hns.com)
  * Emile Stephan (emile.stephan@orange.com)
* **Forum:** QUIC mailing list or GitHub issues on the repo
* **Materials:** 
  * GitHub: https://github.com/NicoKos/QUIC_HIGH_BDP
  * I-D: https://tools.ietf.org/html/draft-kuhn-quic-4-sat-00

### MASQUE
MASQUE is a mechanism that allows co-locating and obfuscating networking applications behind an HTTPS web server. The currently prevalent use-case is to allow running a proxy or VPN server that is indistinguishable from an HTTPS server to any unauthenticated observer. The proxy will be aware of QUIC and will be able to proxy QUIC by having the client and proxy collaborate.

* **Main contact:** David Schinazi <dschinazi.ietf@gmail.com>
* **Forum:** MASQUE IETF mailing list <masque@ietf.org>
* **Materials:**
  * I-D: https://davidschinazi.github.io/masque-drafts/draft-schinazi-masque.html
  * GitHub: https://github.com/DavidSchinazi/masque-drafts

### Multipath
Multipath is an extension enabling QUIC to make use of several network paths concurrently over a single connection. The two main use-cases are bandwidth aggregation and network resiliency.

* **Main contact:**
  * Quentin De Coninck <quentin.deconinck@uclouvain.be>
  * Olivier Bonaventure <olivier.bonaventure@uclouvain.be>
* **Forum:** TBD
* **Materials:**
  * I-D: https://datatracker.ietf.org/doc/draft-deconinck-quic-multipath/
  * GitHub: https://github.com/qdeconinck/draft-deconinck-multipath-quic
  * Blog: https://multipath-quic.org

## Other Proposals

Please create more detailed sections for these above.

* media
* auth handshake
* multicast 
* FEC
* quic-network-sim


## Historic Proposals
