# QPACK Offline Interop

To interop QPACK without a functional QUIC transport, a sequence of HTTP requests or responses can be encoded and serialized to a file.  The file can be decoded by another implementation and compared to the original sequence to verify interop.

Because the QPACK Decoder’s output is normally part of the input to the QPACK encoder via the decoder stream, offline interop requires making assumptions about the decoder’s behavior.  Two modes are possible: 

1. Immediate acknowledgement - the encoder assumes that the decoder acknowledges all header blocks immediately after they are encoded.
2. No acknowledgement - the encoder treats all header blocks as unacknowledged

## Input

HTTP Archive (HAR) containing a sequence of requests or responses.  This is input both to the encoder and the decoder.  

## Parameters

The encoder and decoder must use the same parameters in order to be interoperable.  The parameters are:

1. Dynamic Table Size - the size (in bytes) of the shared dynamic table
2. Max blocked streams - maximum number of blocked streams that the decoder allows
3. Acknowledgement Mode - immediate or none

## Output

The encoder should output a file in the following format.

```
File = Block*
Block = StreamId | Length | QPACKData
StreamId = Stream Id for the block, or 0 for encoder stream data, as a 64 bit integer in network byte order
Length = Length of the QPACKData as 32 bit integer in network byte order
QPACKData = a sequence of octets to be delivered to the QPACK Decoder on the specified stream
```

To simulate out-of-order delivery at the decoder, an encoder can optionally rearrange the blocks in the file (except encoder stream blocks).  For example, when encoding a single header block that generates encoder stream and request stream data, the serialized file may contain the request stream data first.

For convenience, the input HAR file name, implementation name and encoding parameters can be represented in the encoder output filename, eg for input = test1.har: `test1.har.proxygen.4096.100.1`
