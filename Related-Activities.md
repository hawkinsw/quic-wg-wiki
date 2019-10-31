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

### Datagram Frames

Some applications, particularly those that need to transmit real-time data, prefer to transmit data unreliably. These applications can build directly upon UDP as a transport, and can add security with DTLS. Extending QUIC to support transmitting unreliable application data would provide another option for secure datagrams, with the added benefit of sharing a cryptographic and authentication context used for reliable streams.

This proposal defines DATAGRAM QUIC frame types, which carry application data without requiring retransmissions.

* **Main contact:** Tommy Pauly, tpauly@apple.com
* **Forum:** #datagram channel in QUIC slack, or QUIC email list.
* **Materials:** 
  * https://github.com/tfpauly/draft-pauly-quic-datagram
  * https://tools.ietf.org/html/draft-pauly-quic-datagram

### quic-network-simulator

The QUIC Network Simulator is a [ns-3](https://www.nsnam.org/)-based network simulator that allows performance measurements of QUIC implementations under various network conditions. The docker-based setup can be used to run different implementations on the client and the server side. An important use case is running QUIC and TCP traffic across the same link.

* **Main contacts:** 
  * Marten Seemann <martenseemann@gmail.com>
  * Jana Iyengar <jri.ietf@gmail.com>
* **Forum**: #quic-network-sim in the QUIC slack, Github issues on the repo (see below)
* **Materials:** https://github.com/marten-seemann/quic-network-simulator/

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
  * ietf-loss-bits@googlegroups.com (ietf-loss-bits Google Group)
* **Materials:**
  * GitHub: https://github.com/igorlord/draft-ferrieuxhamchaoui-tsvwg-lossbits
  * I-D: https://tools.ietf.org/html/draft-ferrieuxhamchaoui-tsvwg-lossbits
  * Slides: TBD (ETA: Friday)

### 0rtt-bdp

This memo discusses a solution where the client is informed about path parameters: both the client and the server can contribute to the reduction of the time-to-service for subsequent connections. The information can not be seen within the network and is only known at the client and server levels.
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

### WebTransport

WebTransport is a framework for bidirectional communications between web applications and servers.  It provides, among other things, QuicTransport, an API that allows webapps to create a direct QUIC connection to a valid remote server.

* **Main contact:**
  * Victor Vasiliev <vasilvv@google.com>
  * David Schinazi <dschinazi.ietf@gmail.com>
* **Forum:** https://www.ietf.org/mailman/listinfo/webtransport
* **Materials:**
  * https://tools.ietf.org/html/draft-vvv-webtransport-overview
  * https://tools.ietf.org/html/draft-vvv-webtransport-quic
  * https://github.com/WICG/web-transport/blob/master/explainer.md
  * https://wicg.github.io/web-transport/

### Pluginized QUIC

Pluginized QUIC is a framework that enables QUIC clients and servers to dynamically exchange protocol plugins that extend the protocol on a per-connection basis.

* **Main contact:** 
  * Quentin De Coninck <quentin.deconinck@uclouvain.be>
  * François Michel <francois.michel@uclouvain.be>
  * Maxime Piraux <maxime.piraux@uclouvain.be>
  * Olivier Bonaventure <olivier.bonaventure@uclouvain.be>
* **Forum:** TBD
* **Materials:**
  * https://pquic.org/
  * https://github.com/p-quic/

### Performance Enhancement on Mobile networks 

QUIC as a substrate draft (https://datatracker.ietf.org/doc/draft-kuehlewind-quic-substrate/) describes potential use cases where a QUIC proxy is expected in the path between client and server. One of the endpoint (likely the client) is expected to  be directly connected to the QUIC proxy.  This allows endpoints to make explicit decisions about the services they request from the proxies. For example - the access network ( e.g. mobile networks) have often different characteristics than rest of path on the Internet. In order to use the mobile network capacity efficiently, congestion control would need to be optimized differently. A QUIC proxy solution, similar as also proposed for MASQUE, could either assist the endpoint with additional information about the mobile link or enable different congestion control on an outer QUIC connection between that covers the mobile link only. 

* **Main contact:** 
   * Marcus Ihlar <marcus.ihlar@ericsson.com>
   * Mirja Kuehlewind <mirja.kuehlewind@ericsson.com>
   * Zaheduzzaman Sarker <zaheduzzaman.sarker@ericsson.com>
* **Forum:** maque@ietf.org, or etosat@ietf.org for discussion regarding performance enhancements for challenging links
* **Materials:**
  * https://datatracker.ietf.org/doc/draft-kuehlewind-quic-substrate/


## Other Proposals

Please create more detailed sections for these above.

* media
* auth handshake
* multicast 
* FEC


## Historic Proposals