# Overview

For [IETF 110 hackathon](https://trac.ietf.org/trac/ietf/meeting/wiki/110hackathon) we will attempt to do some QUIC performance measurements across different implementations that support the QUIC [perf protocol](https://tools.ietf.org/html/draft-banks-quic-performance). Primarily we will be testing single connection, single (bidirectional) stream throughput (probably upload and download) performance scenarios. Other scenarios may be tested if there is time and/or interest.

# Set up

The MsQuic team will host the hardware (specs [here](https://github.com/microsoft/msquic/wiki/Performance#hardware-specs)) to run the tests. Both Windows (latest Insider builds) and Linux (Ubuntu 20.04) platforms will be supported. Each implementation will provide a client and/or server implementation of the [perf protocol](https://tools.ietf.org/html/draft-banks-quic-performance) along with instructions on how to run it. The MsQuic team will then manually run the performance tests on the matrix of available implementations.

Each implementation will upload a zip of their implementation and instructions to a OneDrive folder (ping @nibanks on Slack for the link).

# Tests

## Single Connection Client Upload

The client will send 1000000000 bytes to the server (zero requested response size) over a single bidirectional stream.

## Single Connection Client Download

The client will request 1000000000 bytes from the server (no other client payload) over a single bidirectional stream.

## Maximum Requests per Second

`TODO`

## Maximum Handshakes per Second

`TODO`

# Results

`TODO`