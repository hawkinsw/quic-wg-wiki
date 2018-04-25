Based on queries from HTTP Archive via Google BigQuery, removing some vendor-proprietary and non-HQ-compatible headers, and adding a few things that wouldn't have been seen in an Archive pass, here's a first pass at an updated static table.
Ordering is by total frequency of the header's occurrence in the archive pass (both requests and responses), and ordering of values is by percentage of responses.  Values are included if they make up more than 5% of the occurrences of that header, minus headers or values that appear to be vendor-specific (user-agent, youtube-client-id, etc.), plus a few (extra status codes, for example).

| Index | Name                             | Value                                                                                                  |
| ----- | -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0 | content-type | application/x-www-form-urlencoded |
| 1 | content-type | image/jpeg |
| 2 | content-type | text/plain;charset=utf-8 |
| 3 | content-type | image/png |
| 4 | content-type | text/plain |
| 5 | content-type | application/json |
| 6 | content-type | application/x-www-form-urlencoded; charset=utf-8 |
| 7 | content-type | image/gif |
| 8 | content-type | text/html; charset=utf-8 |
| 9 | content-type | application/javascript |
| 10 | content-type | text/css |
| 11 | content-type | text/javascript |
| 12 | content-type | application/json; charset=utf-8 |
| 13 | content-type | text/javascript; charset=utf-8 |
| 14 |  |  |
| 15 | content-type | application/x-javascript |
| 16 | server |  |
| 17 | user-agent |  |
| 18 | accept | */* |
| 19 | cache-control | max-age=0 |
| 20 | cache-control | no-cache |
| 21 | cache-control | max-age=2592000 |
| 22 | cache-control | public, max-age=31536000 |
| 23 | cache-control | no-cache, no-store, must-revalidate |
| 24 | cache-control | max-age=604800 |
| 25 | accept-encoding | gzip, deflate, br |
| 26 | accept-encoding | gzip, deflate |
| 27 | vary | accept-encoding |
| 28 | vary | origin |
| 29 | vary | accept-encoding,user-agent |
| 30 | vary | accept |
| 31 | accept-language |  |
| 32 | date |  |
| 33 | referer |  |
| 34 | content-length | 0 |
| 35 | strict-transport-security | max-age=31536000 |
| 36 | strict-transport-security | max-age=10886400; includesubdomains; preload |
| 37 | strict-transport-security | max-age=31536000; includesubdomains |
| 38 | strict-transport-security | max-age=31536000; includesubdomains; preload |
| 39 | :status | 100 |
| 40 | :status | 103 |
| 41 | :status | 200 |
| 42 | :status | 204 |
| 43 | :status | 206 |
| 44 | :status | 302 |
| 45 | :status | 304 |
| 46 | :status | 400 |
| 47 | :status | 403 |
| 48 | :status | 404 |
| 49 | :status | 421 |
| 50 | :status | 500 |
| 51 | :status | 503 |
| 52 | :method | GET |
| 53 | :method | HEAD |
| 54 | :method | POST |
| 55 | :method | PUT |
| 56 | :method | DELETE |
| 57 | :method | CONNECT |
| 58 | :method | OPTIONS |
| 59 | :scheme | http |
| 60 | :scheme | https |
| 61 | :path | / |
| 62 | :path | /index.html |
| 63 | :authority |  |
| 64 | p3p |  |
| 65 | last-modified |  |
| 66 | content-encoding | gzip |
| 67 | content-encoding | br |
| 68 | host |  |
| 69 | expires |  |
| 70 | accept-ranges | bytes |
| 71 | cookie |  |
| 72 | etag |  |
| 73 | alt-svc | clear |
| 74 | x-xss-protection | 1; mode=block |
| 75 | pragma | no-cache |
| 76 | pragma | public |
| 77 | x-powered-by |  |
| 78 | via |  |
| 79 | set-cookie |  |
| 80 | access-control-allow-origin | * |
| 81 | x-content-type-options | nosniff |
| 82 | access-control-allow-headers | content-type |
| 83 | access-control-allow-headers | cache-control |
| 84 | access-control-allow-headers | x-requested-with |
| 85 | access-control-allow-headers | * |
| 86 | access-control-allow-headers | origin, x-requested-with, content-type, accept |
| 87 | access-control-allow-headers | dnt,x-customheader,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 88 | access-control-allow-headers | dnt,x-mx-reqtoken,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 89 | access-control-allow-methods | options |
| 90 | access-control-allow-methods | get |
| 91 | access-control-allow-methods | get, post, options |
| 92 | access-control-allow-methods | get, head, options |
| 93 | access-control-allow-methods | get, options |
| 94 | origin |  |
| 95 | age | 0 |
| 96 | x-frame-options | sameorigin |
| 97 | x-frame-options | deny |
| 98 | timing-allow-origin | * |
| 99 | access-control-expose-headers | content-length |
| 100 | access-control-allow-credentials | TRUE |
| 101 | access-control-allow-credentials | FALSE |
| 102 | expect-ct |  |
| 103 | content-security-policy | script-src 'none'; object-src 'none'; base-uri 'none' |
| 104 | location |  |
| 105 | transfer-encoding | chunked |
| 106 | upgrade-insecure-requests | 1 |
| 107 | link |  |
| 108 | content-disposition | attachment; filename="f.txt" |
| 109 | x-served-by |  |
| 110 | x-requested-with | xmlhttprequest |
| 111 | access-control-request-headers | content-type |
| 112 | access-control-request-headers | x-requested-with |
| 113 | access-control-request-method | post |
| 114 | access-control-request-method | get |
| 115 | range | bytes=0- |
| 116 | if-modified-since |  |
| 117 | purpose | prefetch |
| 118 | if-none-match |  |
| 119 | if-range |  |
| 120 | authorization |  |
| 121 | x-csrf-token | null |
| 122 | x-csrf-token | undefined |