Based on queries from HTTP Archive via Google BigQuery, removing some vendor-proprietary and non-HQ-compatible headers, and adding a few things that wouldn't have been seen in an Archive pass, here's a first pass at an updated static table.
Ordering is by total frequency of the header's occurrence in the archive pass (both requests and responses), and ordering of values is by percentage of responses.  Values are included if they make up more than 5% of the occurrences of that header, minus headers or values that appear to be vendor-specific (user-agent, youtube-client-id, etc.), plus a few (extra status codes, for example).

| Index | Name                             | Value                                                                                                  |
| ----- | -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0 | :path | / |
| 1 | referer | |
| 2 | content-length | 0 |
| 3 | accept-encoding | gzip, deflate, br |
| 4 | accept-language |  |
| 5 | date |  |
| 6 | last-modified |  |
| 7 | :authority |  |
| 8 | expires |  |
| 9 | cookie |  |
| 10 | etag |  |
| 11 | location |  |
| 12 | if-modified-since |  |
| 13 | if-none-match |  |
| 14 | set-cookie |  |
| 15 | link |  |
| 16 | content-disposition | attachment; filename="f.txt" |
| 17 | range | bytes=0- |
| 18 | :scheme | https |
| 19 | :scheme | http |
| 20 | :status | 200 |
| 21 | :status | 304 |
| 22 | :path | /index.html |
| 23 | :method | GET |
| 24 | :method | HEAD |
| 25 | :method | POST |
| 26 | :method | PUT |
| 27 | :method | DELETE |
| 28 | :method | CONNECT |
| 29 | :method | OPTIONS |
| 30 | content-type | application/x-www-form-urlencoded |
| 31 | content-type | image/jpeg |
| 32 | content-type | text/plain;charset=utf-8 |
| 33 | content-type | image/png |
| 34 | content-type | text/plain |
| 35 | content-type | application/json |
| 36 | content-type | application/x-www-form-urlencoded; charset=utf-8 |
| 37 | content-type | image/gif |
| 38 | content-type | text/html; charset=utf-8 |
| 39 | content-type | application/javascript |
| 40 | content-type | text/css |
| 41 | content-type | text/javascript |
| 42 | content-type | application/json; charset=utf-8 |
| 43 | content-type | text/javascript; charset=utf-8 |
| 44 | content-type | application/x-javascript |
| 45 | server |  |
| 46 | user-agent |  |
| 47 | accept | */* |
| 48 | cache-control | max-age=0 |
| 49 | cache-control | no-cache |
| 50 | cache-control | max-age=2592000 |
| 51 | cache-control | public, max-age=31536000 |
| 52 | cache-control | no-cache, no-store, must-revalidate |
| 53 | cache-control | max-age=604800 |
| 54 | accept-encoding | gzip, deflate |
| 55 | vary | accept-encoding |
| 56 | vary | origin |
| 57 | vary | accept-encoding,user-agent |
| 58 | vary | accept |
| 59 | strict-transport-security | max-age=31536000 |
| 60 | strict-transport-security | max-age=10886400; includesubdomains; preload |
| 61 | strict-transport-security | max-age=31536000; includesubdomains |
| 62 | strict-transport-security | max-age=31536000; includesubdomains; preload |
| 63 | :status | 100 |
| 64 | :status | 103 |
| 65 | :status | 204 |
| 66 | :status | 206 |
| 67 | :status | 302 |
| 68 | :status | 400 |
| 69 | :status | 403 |
| 70 | :status | 404 |
| 71 | :status | 421 |
| 72 | :status | 500 |
| 73 | :status | 503 |
| 74 | p3p |  |
| 75 | content-encoding | gzip |
| 76 | content-encoding | br |
| 77 | accept-ranges | bytes |
| 78 | alt-svc | clear |
| 79 | x-xss-protection | 1; mode=block |
| 80 | host |  |
| 81 | pragma | no-cache |
| 82 | pragma | public |
| 83 | x-powered-by |  |
| 84 | via |  |
| 85 | access-control-allow-origin | * |
| 86 | x-content-type-options | nosniff |
| 87 | access-control-allow-headers | content-type |
| 88 | access-control-allow-headers | cache-control |
| 89 | access-control-allow-headers | x-requested-with |
| 90 | access-control-allow-headers | * |
| 91 | access-control-allow-headers | origin, x-requested-with, content-type, accept |
| 92 | access-control-allow-headers | dnt,x-customheader,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 93 | access-control-allow-headers | dnt,x-mx-reqtoken,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 94 | access-control-allow-methods | options |
| 95 | access-control-allow-methods | get |
| 96 | access-control-allow-methods | get, post, options |
| 97 | access-control-allow-methods | get, head, options |
| 98 | access-control-allow-methods | get, options |
| 99 | origin |  |
| 100 | age | 0 |
| 101 | x-frame-options | sameorigin |
| 102 | x-frame-options | deny |
| 103 | timing-allow-origin | * |
| 104 | access-control-expose-headers | content-length |
| 105 | access-control-allow-credentials | TRUE |
| 106 | access-control-allow-credentials | FALSE |
| 107 | expect-ct |  |
| 108 | content-security-policy | script-src 'none'; object-src 'none'; base-uri 'none' |
| 109 | transfer-encoding | chunked |
| 110 | upgrade-insecure-requests | 1 |
| 111 | x-served-by |  |
| 112 | x-requested-with | xmlhttprequest |
| 113 | access-control-request-headers | content-type |
| 114 | access-control-request-headers | x-requested-with |
| 115 | access-control-request-method | post |
| 116 | access-control-request-method | get |
| 117 | purpose | prefetch |
| 118 | if-range |  |
| 119 | authorization |  |
| 120 | x-csrf-token | null |
| 121 | x-csrf-token | undefined |