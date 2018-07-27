To interop QPACK without a functional QUIC transport, a sequence of HTTP requests or responses can be encoded and serialized to a file.  The file can be decoded by another implementation and compared to the original sequence to verify interop.

Because the QPACK Decoder’s output is normally part of the input to the QPACK encoder via the decoder stream, offline interop requires making assumptions about the decoder’s behavior.  Two modes are possible: 

1. Immediate acknowledgement - the encoder assumes that the decoder acknowledges all header blocks immediately after they are encoded.
2. No acknowledgement - the encoder treats all header blocks as unacknowledged

## Interop Overview

The encoder program consumes a list of header sets from a QIF file and produces an intermediate output that contains header blocks and encoder stream instructions.  The decoder reads this file and outputs its own list of header set in QIF format.  If the encoder input and the decoder output are the same, the interop is successful.

## The QIF Format

QIF stands for _QPACK Interop Format_.  It is meant to be easy to parse, write, and compare.  The QIF format is plain text.  Each header field in on a separate line,# with name and value separated by the TAB character.  An empty line signifies the end of a header set.  Lines beginning with '#' are ignored.  Example:

```
# I am a QIF file
:method GET
:scheme http
:authority      www.netbsd.org
:path   /
user-agent      Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:58.0) Gecko/20100101 Firefox/58.0
accept  text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
accept-language en-US,en;q=0.5
accept-encoding gzip, deflate
connection      keep-alive
upgrade-insecure-requests       1
pragma  no-cache
cache-control   no-cache

:method GET
:scheme http
:authority      www.netbsd.org
:path   /global.css
user-agent      Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:58.0) Gecko/20100101 Firefox/58.0
accept  text/css,*/*;q=0.1
accept-language en-US,en;q=0.5
accept-encoding gzip, deflate
referer http://www.netbsd.org/
connection      keep-alive
pragma  no-cache
cache-control   no-cache

```

## Generating QIF Files

One can create QIF files manually using a text editor.  An easier way to get a bunch of real-life headers is to record a web session as an HTTP Archive (HAR) and then convert the HAR to a QIF file using the [har2qif.pl tool](https://github.com/litespeedtech/ls-qpack/blob/master/tools/har2qif.pl).

## Parameters

The encoder and decoder must use the same parameters in order to be interoperable.  The parameters are:

1. Dynamic Table Size - the size (in bytes) of the shared dynamic table
2. Max blocked streams - maximum number of blocked streams that the decoder allows
3. Acknowledgement Mode - immediate or none

## Encoder Output / Decoder Input

The encoder should output a file in the following format.

```
File = Block*
Block = StreamId | Length | QPACKData
StreamId = Stream Id for the block, or 0 for encoder stream data, as a 64 bit integer in network byte order
Length = Length of the QPACKData as 32 bit integer in network byte order
QPACKData = a sequence of octets to be delivered to the QPACK Decoder on the specified stream
```

Each header set in the input QIF file is a header block with a new stream ID.  For the purposes of the Interop, streams IDs begin at 1 and increase by 1 (1, 2, 3,..), while stream ID 0 is reserved to indicate the encoder stream.

To simulate out-of-order delivery at the decoder, an encoder can optionally rearrange the blocks in the file (except encoder stream blocks).  For example, when encoding a single header block that generates encoder stream and request stream data, the serialized file may contain the request stream data first.

For convenience, the input HAR file name, implementation name and encoding parameters can be represented in the encoder output filename, eg for input = test1.har: `test1.har.proxygen.4096.100.1`
