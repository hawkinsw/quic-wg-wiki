- Features in 2nd implementation draft

- Update to draft-08 and TLS 1.3 -22

- Resumption
  - Successful resumption
  - Rejected resumption

- 0-RTT
  - Successful 0-RTT
  - 0-RTT rejection and retransmit
  - Loss of 0-RTT data and retransmit
  
- Functional loss recovery. The target here is that the operation eventually succeeds, not
that it succeeds within any particular time frame. I.e., timeout
based loss recovery is fine. Test cases:

  - Drop Initial
  - Drop all of server first flight
  - Have a certificate large enough to have two packets in
    server first flight, drop P1 and P2
  - Drop client second flight (generally one datagram)
  - Transfer a file of 1 MB with random 10% loss



    