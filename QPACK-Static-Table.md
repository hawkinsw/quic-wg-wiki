*NB: This page reflects early thinking on a new QPACK static table. The below text was formed up into [Pull Request #1355](https://github.com/quicwg/base-drafts/pull/1355), additional discussion took place on [Issue #904](https://github.com/quicwg/base-drafts/issues/904)*

Based on queries from HTTP Archive via Google BigQuery, removing some vendor-proprietary and non-HQ-compatible headers, and adding a few things that wouldn't have been seen in an Archive pass, here's a first pass at an updated static table.
Ordering is by total frequency of the header's occurrence in the archive pass (both requests and responses), and ordering of values is by percentage of responses.  Values are included if they make up more than 5% of the occurrences of that header, minus headers or values that appear to be vendor-specific (user-agent, youtube-client-id, etc.), plus a few (extra status codes, for example).

| Index | Name                             | Value                                                                                                  |
| ----- | -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0 | :path | / |
| 1 | referer |  |
| 2 | content-length | 0 |
| 3 | date |  |
| 4 | last-modified |  |
| 5 | :authority |  |
| 6 | expires |  |
| 7 | cookie |  |
| 8 | etag |  |
| 9 | location |  |
| 10 | if-modified-since |  |
| 11 | if-none-match |  |
| 12 | if-range |  |
| 13 | set-cookie |  |
| 14 | age | 0 |
| 15 | link |  |
| 16 | content-disposition | attachment; filename="f.txt" |
| 17 | range | bytes=0- |
| 18 | authorization |  |
| 19 | :scheme | https |
| 20 | :scheme | http |
| 21 | :status | 200 |
| 22 | :status | 304 |
| 23 | :path | /index.html |
| 24 | :method | GET |
| 25 | :method | HEAD |
| 26 | :method | POST |
| 27 | :method | PUT |
| 28 | :method | DELETE |
| 29 | :method | CONNECT |
| 30 | :method | OPTIONS |
| 31 | content-type | application/x-www-form-urlencoded |
| 32 | content-type | image/jpeg |
| 33 | content-type | text/plain;charset=utf-8 |
| 34 | content-type | image/png |
| 35 | content-type | text/plain |
| 36 | content-type | application/json |
| 37 | content-type | application/x-www-form-urlencoded; charset=utf-8 |
| 38 | content-type | image/gif |
| 39 | content-type | text/html; charset=utf-8 |
| 40 | content-type | application/javascript |
| 41 | content-type | text/css |
| 42 | content-type | text/javascript |
| 43 | content-type | application/json; charset=utf-8 |
| 44 | content-type | text/javascript; charset=utf-8 |
| 45 | content-type | application/x-javascript |
| 46 | server |  |
| 47 | user-agent |  |
| 48 | accept | */* |
| 49 | cache-control | max-age=0 |
| 50 | cache-control | no-cache |
| 51 | cache-control | max-age=2592000 |
| 52 | cache-control | public, max-age=31536000 |
| 53 | cache-control | no-cache, no-store, must-revalidate |
| 54 | cache-control | max-age=604800 |
| 55 | accept-encoding | gzip, deflate, br |
| 56 | accept-encoding | gzip, deflate |
| 57 | vary | accept-encoding |
| 58 | vary | origin |
| 59 | vary | accept-encoding,user-agent |
| 60 | vary | accept |
| 61 | accept-language |  |
| 62 | strict-transport-security | max-age=31536000 |
| 63 | strict-transport-security | max-age=10886400; includesubdomains; preload |
| 64 | strict-transport-security | max-age=31536000; includesubdomains |
| 65 | strict-transport-security | max-age=31536000; includesubdomains; preload |
| 66 | :status | 100 |
| 67 | :status | 103 |
| 68 | :status | 204 |
| 69 | :status | 206 |
| 70 | :status | 302 |
| 71 | :status | 400 |
| 72 | :status | 403 |
| 73 | :status | 404 |
| 74 | :status | 421 |
| 75 | :status | 500 |
| 76 | :status | 503 |
| 77 | p3p |  |
| 78 | content-encoding | gzip |
| 79 | content-encoding | br |
| 80 | accept-ranges | bytes |
| 81 | alt-svc | clear |
| 82 | x-xss-protection | 1; mode=block |
| 83 | host |  |
| 84 | pragma | no-cache |
| 85 | pragma | public |
| 86 | x-powered-by |  |
| 87 | via |  |
| 88 | access-control-allow-origin | * |
| 89 | x-content-type-options | nosniff |
| 90 | access-control-allow-headers | content-type |
| 91 | access-control-allow-headers | cache-control |
| 92 | access-control-allow-headers | x-requested-with |
| 93 | access-control-allow-headers | * |
| 94 | access-control-allow-headers | origin, x-requested-with, content-type, accept |
| 95 | access-control-allow-headers | dnt,x-customheader,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 96 | access-control-allow-headers | dnt,x-mx-reqtoken,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 97 | access-control-allow-methods | options |
| 98 | access-control-allow-methods | get |
| 99 | access-control-allow-methods | get, post, options |
| 100 | access-control-allow-methods | get, head, options |
| 101 | access-control-allow-methods | get, options |
| 102 | origin |  |
| 103 | x-frame-options | sameorigin |
| 104 | x-frame-options | deny |
| 105 | timing-allow-origin | * |
| 106 | access-control-expose-headers | content-length |
| 107 | access-control-allow-credentials | TRUE |
| 108 | access-control-allow-credentials | FALSE |
| 109 | expect-ct |  |
| 110 | content-security-policy | script-src 'none'; object-src 'none'; base-uri 'none' |
| 111 | transfer-encoding | chunked |
| 112 | upgrade-insecure-requests | 1 |
| 113 | x-served-by |  |
| 114 | x-requested-with | xmlhttprequest |
| 115 | access-control-request-headers | content-type |
| 116 | access-control-request-headers | x-requested-with |
| 117 | access-control-request-method | post |
| 118 | access-control-request-method | get |
| 119 | purpose | prefetch |
| 120 | x-csrf-token | null |
| 121 | x-csrf-token | undefined |