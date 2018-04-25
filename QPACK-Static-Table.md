Based on queries from HTTP Archive via Google BigQuery, removing some vendor-proprietary and non-HQ-compatible headers, and adding a few things that wouldn't have been seen in an Archive pass, here's a first pass at an updated static table.
Ordering is by total frequency of the header's occurrence in the archive pass (both requests and responses), and ordering of values is by percentage of responses.  Values are included if they make up more than 5% of the occurrences of that header, minus headers or values that appear to be vendor-specific (user-agent, youtube-client-id, etc.), plus a few (extra status codes, for example).

| Index | Name                             | Value                                                                                                  |
| ----- | -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0     | content-type                     | application/x-www-form-urlencoded                                                                      |
| 1     | content-type                     | image/jpeg                                                                                             |
| 2     | content-type                     | text/plain;charset=utf-8                                                                               |
| 3     | content-type                     | image/png                                                                                              |
| 4     | content-type                     | text/plain                                                                                             |
| 5     | content-type                     | application/json                                                                                       |
| 6     | content-type                     | application/x-www-form-urlencoded; charset=utf-8                                                       |
| 7     | content-type                     | image/gif                                                                                              |
| 8     | content-type                     | text/html; charset=utf-8                                                                               |
| 9     | content-type                     | application/javascript                                                                                 |
| 10    | content-type                     | text/css                                                                                               |
| 11    | content-type                     | text/javascript                                                                                        |
| 12    | content-type                     | application/json; charset=utf-8                                                                        |
| 13    | content-type                     | text/javascript; charset=utf-8                                                                         |
| 14    | content-type                     | application/x-javascript                                                                               |
| 15    | server                           |                                                                                                        |
| 16    | user-agent                       |                                                                                                        |
| 17    | accept                           | */*                                                                                                    |
| 18    | cache-control                    | max-age=0                                                                                              |
| 19    | cache-control                    | no-cache                                                                                               |
| 20    | cache-control                    | max-age=2592000                                                                                        |
| 21    | cache-control                    | public, max-age=31536000                                                                               |
| 22    | cache-control                    | no-cache, no-store, must-revalidate                                                                    |
| 23    | cache-control                    | max-age=604800                                                                                         |
| 24    | accept-encoding                  | gzip, deflate, br                                                                                      |
| 25    | accept-encoding                  | gzip, deflate                                                                                          |
| 26    | vary                             | accept-encoding                                                                                        |
| 27    | vary                             | origin                                                                                                 |
| 28    | vary                             | accept-encoding,user-agent                                                                             |
| 29    | vary                             | accept                                                                                                 |
| 30    | accept-language                  |                                                                                                        |
| 31    | date                             |                                                                                                        |
| 32    | referer                          |                                                                                                        |
| 33    | content-length                   | 0                                                                                                      |
| 34    | strict-transport-security        | max-age=31536000                                                                                       |
| 35    | strict-transport-security        | max-age=10886400; includesubdomains; preload                                                           |
| 36    | strict-transport-security        | max-age=31536000; includesubdomains                                                                    |
| 37    | strict-transport-security        | max-age=31536000; includesubdomains; preload                                                           |
| 38    | :status                          | 100                                                                                                    |
| 39    | :status                          | 103                                                                                                    |
| 40    | :status                          | 200                                                                                                    |
| 41    | :status                          | 204                                                                                                    |
| 42    | :status                          | 206                                                                                                    |
| 43    | :status                          | 302                                                                                                    |
| 44    | :status                          | 304                                                                                                    |
| 45    | :status                          | 400                                                                                                    |
| 46    | :status                          | 403                                                                                                    |
| 47    | :status                          | 404                                                                                                    |
| 48    | :status                          | 421                                                                                                    |
| 49    | :status                          | 500                                                                                                    |
| 50    | :status                          | 503                                                                                                    |
| 51    | p3p                              |                                                                                                        |
| 52    | last-modified                    |                                                                                                        |
| 53    | content-encoding                 | gzip                                                                                                   |
| 54    | content-encoding                 | br                                                                                                     |
| 55    | host                             |                                                                                                        |
| 56    | expires                          |                                                                                                        |
| 57    | accept-ranges                    | bytes                                                                                                  |
| 58    | cookie                           |                                                                                                        |
| 59    | etag                             |                                                                                                        |
| 60    | alt-svc                          | clear                                                                                                  |
| 61    | x-xss-protection                 | 1; mode=block                                                                                          |
| 62    | pragma                           | no-cache                                                                                               |
| 63    | pragma                           | public                                                                                                 |
| 64    | x-powered-by                     |                                                                                                        |
| 65    | via                              |                                                                                                        |
| 66    | set-cookie                       |                                                                                                        |
| 67    | access-control-allow-origin      | *                                                                                                      |
| 68    | x-content-type-options           | nosniff                                                                                                |
| 69    | access-control-allow-headers     | content-type                                                                                           |
| 70    | access-control-allow-headers     | cache-control                                                                                          |
| 71    | access-control-allow-headers     | x-requested-with                                                                                       |
| 72    | access-control-allow-headers     | *                                                                                                      |
| 73    | access-control-allow-headers     | origin, x-requested-with, content-type, accept                                                         |
| 74    | access-control-allow-headers     | dnt,x-customheader,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| 75    | access-control-allow-headers     | dnt,x-mx-reqtoken,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type  |
| 76    | access-control-allow-methods     | options                                                                                                |
| 77    | access-control-allow-methods     | get                                                                                                    |
| 78    | access-control-allow-methods     | get, post, options                                                                                     |
| 79    | access-control-allow-methods     | get, head, options                                                                                     |
| 80    | access-control-allow-methods     | get, options                                                                                           |
| 81    | origin                           |                                                                                                        |
| 82    | age                              | 0                                                                                                      |
| 83    | x-frame-options                  | sameorigin                                                                                             |
| 84    | x-frame-options                  | deny                                                                                                   |
| 85    | timing-allow-origin              | *                                                                                                      |
| 86    | access-control-expose-headers    | content-length                                                                                         |
| 87    | access-control-allow-credentials | TRUE                                                                                                   |
| 88    | access-control-allow-credentials | FALSE                                                                                                  |
| 89    | expect-ct                        |                                                                                                        |
| 90    | content-security-policy          | script-src 'none'; object-src 'none'; base-uri 'none'                                                  |
| 91    | location                         |                                                                                                        |
| 92    | transfer-encoding                | chunked                                                                                                |
| 93    | upgrade-insecure-requests        | 1                                                                                                      |
| 94    | link                             |                                                                                                        |
| 95    | content-disposition              | attachment; filename="f.txt"                                                                           |
| 96    | x-served-by                      |                                                                                                        |
| 97    | x-requested-with                 | xmlhttprequest                                                                                         |
| 98    | access-control-request-headers   | content-type                                                                                           |
| 99    | access-control-request-headers   | x-requested-with                                                                                       |
| 100   | access-control-request-method    | post                                                                                                   |
| 101   | access-control-request-method    | get                                                                                                    |
| 102   | range                            | bytes=0-                                                                                               |
| 103   | if-modified-since                |                                                                                                        |
| 104   | purpose                          | prefetch                                                                                               |
| 105   | if-none-match                    |                                                                                                        |
| 106   | if-range                         |                                                                                                        |
| 107   | authorization                    |                                                                                                        |
| 108   | x-csrf-token                     | null                                                                                                   |
| 109   | x-csrf-token                     | undefined                                                                                              |