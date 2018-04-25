Based on queries from HTTP Archive via Google BigQuery, removing some vendor-proprietary and non-HQ-compatible headers, and adding a few things that wouldn't have been seen in an Archive pass, here's a first pass at an updated static table.
Ordering is by total frequency of the header's occurrence in the archive pass (both requests and responses), and ordering of values is by percentage of responses.  Values are included if they make up more than 5% of the occurrences of that header, minus headers or values that appear to be vendor-specific (user-agent, youtube-client-id, etc.), plus a few (extra status codes, for example).

| Name                             | Value                                                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| content-type                     | application/x-www-form-urlencoded                                                                      |
| content-type                     | image/jpeg                                                                                             |
| content-type                     | text/plain;charset=utf-8                                                                               |
| content-type                     | image/png                                                                                              |
| content-type                     | text/plain                                                                                             |
| content-type                     | application/json                                                                                       |
| content-type                     | application/x-www-form-urlencoded; charset=utf-8                                                       |
| content-type                     | image/gif                                                                                              |
| content-type                     | text/html; charset=utf-8                                                                               |
| content-type                     | application/javascript                                                                                 |
| content-type                     | text/css                                                                                               |
| content-type                     | text/javascript                                                                                        |
| content-type                     | application/json; charset=utf-8                                                                        |
| content-type                     | text/javascript; charset=utf-8                                                                         |
| content-type                     | application/x-javascript                                                                               |
| server                           |                                                                                                        |
| user-agent                       |                                                                                                        |
| accept                           | */*                                                                                                    |
| cache-control                    | max-age=0                                                                                              |
| cache-control                    | no-cache                                                                                               |
| cache-control                    | max-age=2592000                                                                                        |
| cache-control                    | public, max-age=31536000                                                                               |
| cache-control                    | no-cache, no-store, must-revalidate                                                                    |
| cache-control                    | max-age=604800                                                                                         |
| accept-encoding                  | gzip, deflate, br                                                                                      |
| accept-encoding                  | gzip, deflate                                                                                          |
| vary                             | accept-encoding                                                                                        |
| vary                             | origin                                                                                                 |
| vary                             | accept-encoding,user-agent                                                                             |
| vary                             | accept                                                                                                 |
| accept-language                  |                                                                                                        |
| date                             |                                                                                                        |
| referer                          |                                                                                                        |
| content-length                   | 0                                                                                                      |
| strict-transport-security        | max-age=31536000                                                                                       |
| strict-transport-security        | max-age=10886400; includesubdomains; preload                                                           |
| strict-transport-security        | max-age=31536000; includesubdomains                                                                    |
| strict-transport-security        | max-age=31536000; includesubdomains; preload                                                           |
| :status                          | 100                                                                                                    |
| :status                          | 103                                                                                                    |
| :status                          | 200                                                                                                    |
| :status                          | 204                                                                                                    |
| :status                          | 206                                                                                                    |
| :status                          | 302                                                                                                    |
| :status                          | 304                                                                                                    |
| :status                          | 400                                                                                                    |
| :status                          | 403                                                                                                    |
| :status                          | 404                                                                                                    |
| :status                          | 421                                                                                                    |
| :status                          | 500                                                                                                    |
| :status                          | 503                                                                                                    |
| p3p                              |                                                                                                        |
| last-modified                    |                                                                                                        |
| content-encoding                 | gzip                                                                                                   |
| content-encoding                 | br                                                                                                     |
| host                             |                                                                                                        |
| expires                          |                                                                                                        |
| accept-ranges                    | bytes                                                                                                  |
| cookie                           |                                                                                                        |
| etag                             |                                                                                                        |
| alt-svc                          | clear                                                                                                  |
| x-xss-protection                 | 1; mode=block                                                                                          |
| pragma                           | no-cache                                                                                               |
| pragma                           | public                                                                                                 |
| x-powered-by                     |                                                                                                        |
| via                              |                                                                                                        |
| set-cookie                       |                                                                                                        |
| access-control-allow-origin      | *                                                                                                      |
| x-content-type-options           | nosniff                                                                                                |
| access-control-allow-headers     | content-type                                                                                           |
| access-control-allow-headers     | cache-control                                                                                          |
| access-control-allow-headers     | x-requested-with                                                                                       |
| access-control-allow-headers     | *                                                                                                      |
| access-control-allow-headers     | origin, x-requested-with, content-type, accept                                                         |
| access-control-allow-headers     | dnt,x-customheader,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type |
| access-control-allow-headers     | dnt,x-mx-reqtoken,keep-alive,user-agent,x-requested-with,if-modified-since,cache-control,content-type  |
| access-control-allow-methods     | options                                                                                                |
| access-control-allow-methods     | get                                                                                                    |
| access-control-allow-methods     | get, post, options                                                                                     |
| access-control-allow-methods     | get, head, options                                                                                     |
| access-control-allow-methods     | get, options                                                                                           |
| origin                           |                                                                                                        |
| age                              | 0                                                                                                      |
| x-frame-options                  | sameorigin                                                                                             |
| x-frame-options                  | deny                                                                                                   |
| timing-allow-origin              | *                                                                                                      |
| access-control-expose-headers    | content-length                                                                                         |
| access-control-allow-credentials | TRUE                                                                                                   |
| access-control-allow-credentials | FALSE                                                                                                  |
| expect-ct                        |                                                                                                        |
| content-security-policy          | script-src 'none'; object-src 'none'; base-uri 'none'                                                  |
| location                         |                                                                                                        |
| transfer-encoding                | chunked                                                                                                |
| upgrade-insecure-requests        | 1                                                                                                      |
| link                             |                                                                                                        |
| content-disposition              | attachment; filename="f.txt"                                                                           |
| x-served-by                      |                                                                                                        |
| x-requested-with                 | xmlhttprequest                                                                                         |
| access-control-request-headers   | content-type                                                                                           |
| access-control-request-headers   | x-requested-with                                                                                       |
| access-control-request-method    | post                                                                                                   |
| access-control-request-method    | get                                                                                                    |
| range                            | bytes=0-                                                                                               |
| if-modified-since                |                                                                                                        |
| purpose                          | prefetch                                                                                               |
| if-none-match                    |                                                                                                        |
| if-range                         |                                                                                                        |
| authorization                    |                                                                                                        |
| x-csrf-token                     | null                                                                                                   |
| x-csrf-token                     | undefined                                                                                              |
