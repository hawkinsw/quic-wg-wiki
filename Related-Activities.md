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
* **Materials:** [[https://github.com/martinduke/draft-duke-quic-load-balancers]]

### Other proposals

* MASQUE
* datagram
* media
* multipath
* lossbits
* auth handshake
* 0rtt-bdp
* multicast 
* satellite / etosat
* FEC
* qlog
* quic-network-sim


## Historic Proposals

