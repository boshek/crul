<!--
%\VignetteIndexEntry{1. crul introduction}
%\VignetteEngine{knitr::rmarkdown}
%\VignetteEncoding{UTF-8}
-->



crul introduction
=================

`crul` is an HTTP client for R.

## Install

Stable CRAN version


```r
install.packages("crul")
```

Dev version


```r
devtools::install_github("ropensci/crul")
```


```r
library("crul")
```

## the client

`HttpClient` is where to start


```r
(x <- HttpClient$new(
  url = "https://httpbin.org",
  opts = list(
    timeout = 1
  ),
  headers = list(
    a = "hello world"
  )
))
#> <crul connection> 
#>   url: https://httpbin.org
#>   curl options: 
#>     timeout: 1
#>   proxies: 
#>   auth: 
#>   headers: 
#>     a: hello world
#>   progress: FALSE
#>   hooks:
```

Makes a R6 class, that has all the bits and bobs you'd expect for doing HTTP
requests. When it prints, it gives any defaults you've set. As you update
the object you can see what's been set


```r
x$opts
#> $timeout
#> [1] 1
```


```r
x$headers
#> $a
#> [1] "hello world"
```

## do some http

The client object created above has http methods that you can call,
and pass paths to, as well as query parameters, body values, and any other
curl options.

Here, we'll do a __GET__ request on the route `/get` on our base url
`https://httpbin.org` (the full url is then `https://httpbin.org/get`)


```r
res <- x$get("get")
```

The response from a http request is another R6 class `HttpResponse`, which
has slots for the outputs of the request, and some functions to deal with
the response:

Status code


```r
res$status_code
#> [1] 200
```

The content


```r
res$content
#>   [1] 7b 0a 20 20 22 61 72 67 73 22 3a 20 7b 7d 2c 20 0a 20 20 22 68 65 61
#>  [24] 64 65 72 73 22 3a 20 7b 0a 20 20 20 20 22 41 22 3a 20 22 68 65 6c 6c
#>  [47] 6f 20 77 6f 72 6c 64 22 2c 20 0a 20 20 20 20 22 41 63 63 65 70 74 22
#>  [70] 3a 20 22 61 70 70 6c 69 63 61 74 69 6f 6e 2f 6a 73 6f 6e 2c 20 74 65
#>  [93] 78 74 2f 78 6d 6c 2c 20 61 70 70 6c 69 63 61 74 69 6f 6e 2f 78 6d 6c
#> [116] 2c 20 2a 2f 2a 22 2c 20 0a 20 20 20 20 22 41 63 63 65 70 74 2d 45 6e
#> [139] 63 6f 64 69 6e 67 22 3a 20 22 67 7a 69 70 2c 20 64 65 66 6c 61 74 65
#> [162] 22 2c 20 0a 20 20 20 20 22 48 6f 73 74 22 3a 20 22 68 74 74 70 62 69
#> [185] 6e 2e 6f 72 67 22 2c 20 0a 20 20 20 20 22 55 73 65 72 2d 41 67 65 6e
#> [208] 74 22 3a 20 22 6c 69 62 63 75 72 6c 2f 37 2e 35 34 2e 30 20 72 2d 63
#> [231] 75 72 6c 2f 33 2e 33 20 63 72 75 6c 2f 30 2e 37 2e 34 22 0a 20 20 7d
#> [254] 2c 20 0a 20 20 22 6f 72 69 67 69 6e 22 3a 20 22 36 35 2e 31 39 37 2e
#> [277] 31 34 36 2e 31 38 2c 20 36 35 2e 31 39 37 2e 31 34 36 2e 31 38 22 2c
#> [300] 20 0a 20 20 22 75 72 6c 22 3a 20 22 68 74 74 70 73 3a 2f 2f 68 74 74
#> [323] 70 62 69 6e 2e 6f 72 67 2f 67 65 74 22 0a 7d 0a
```

HTTP method


```r
res$method
#> [1] "get"
```

Request headers


```r
res$request_headers
#> $`User-Agent`
#> [1] "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"
#> 
#> $`Accept-Encoding`
#> [1] "gzip, deflate"
#> 
#> $Accept
#> [1] "application/json, text/xml, application/xml, */*"
#> 
#> $a
#> [1] "hello world"
```

Response headers


```r
res$response_headers
#> $status
#> [1] "HTTP/1.1 200 OK"
#> 
#> $`access-control-allow-credentials`
#> [1] "true"
#> 
#> $`access-control-allow-origin`
#> [1] "*"
#> 
#> $`content-encoding`
#> [1] "gzip"
#> 
#> $`content-type`
#> [1] "application/json"
#> 
#> $date
#> [1] "Thu, 28 Mar 2019 00:00:11 GMT"
#> 
#> $server
#> [1] "nginx"
#> 
#> $`content-length`
#> [1] "228"
#> 
#> $connection
#> [1] "keep-alive"
```

All response headers, including intermediate headers, if any


```r
res$response_headers_all
#> [[1]]
#> [[1]]$status
#> [1] "HTTP/1.1 200 OK"
#> 
#> [[1]]$`access-control-allow-credentials`
#> [1] "true"
#> 
#> [[1]]$`access-control-allow-origin`
#> [1] "*"
#> 
#> [[1]]$`content-encoding`
#> [1] "gzip"
#> 
#> [[1]]$`content-type`
#> [1] "application/json"
#> 
#> [[1]]$date
#> [1] "Thu, 28 Mar 2019 00:00:11 GMT"
#> 
#> [[1]]$server
#> [1] "nginx"
#> 
#> [[1]]$`content-length`
#> [1] "228"
#> 
#> [[1]]$connection
#> [1] "keep-alive"
```

And you can parse the content with a provided function:


```r
res$parse()
#> [1] "{\n  \"args\": {}, \n  \"headers\": {\n    \"A\": \"hello world\", \n    \"Accept\": \"application/json, text/xml, application/xml, */*\", \n    \"Accept-Encoding\": \"gzip, deflate\", \n    \"Host\": \"httpbin.org\", \n    \"User-Agent\": \"libcurl/7.54.0 r-curl/3.3 crul/0.7.4\"\n  }, \n  \"origin\": \"65.197.146.18, 65.197.146.18\", \n  \"url\": \"https://httpbin.org/get\"\n}\n"
jsonlite::fromJSON(res$parse())
#> $args
#> named list()
#> 
#> $headers
#> $headers$A
#> [1] "hello world"
#> 
#> $headers$Accept
#> [1] "application/json, text/xml, application/xml, */*"
#> 
#> $headers$`Accept-Encoding`
#> [1] "gzip, deflate"
#> 
#> $headers$Host
#> [1] "httpbin.org"
#> 
#> $headers$`User-Agent`
#> [1] "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"
#> 
#> 
#> $origin
#> [1] "65.197.146.18, 65.197.146.18"
#> 
#> $url
#> [1] "https://httpbin.org/get"
```

With the `HttpClient` object, which holds any configuration stuff
we set, we can make other HTTP verb requests. For example, a `HEAD`
request:


```r
x$post(
  path = "post", 
  body = list(hello = "world")
)
```


## write to disk


```r
x <- HttpClient$new(url = "https://httpbin.org")
f <- tempfile()
res <- x$get(disk = f)
# when using write to disk, content is a path
res$content 
#> [1] "/var/folders/fc/n7g_vrvn0sx_st0p8lxb3ts40000gn/T//RtmpEWu0Rm/file13adb42c22155"
```

Read lines


```r
readLines(res$content, n = 10)
#>  [1] "<!DOCTYPE html>"                                                                                                               
#>  [2] "<html lang=\"en\">"                                                                                                            
#>  [3] ""                                                                                                                              
#>  [4] "<head>"                                                                                                                        
#>  [5] "    <meta charset=\"UTF-8\">"                                                                                                  
#>  [6] "    <title>httpbin.org</title>"                                                                                                
#>  [7] "    <link href=\"https://fonts.googleapis.com/css?family=Open+Sans:400,700|Source+Code+Pro:300,600|Titillium+Web:400,600,700\""
#>  [8] "        rel=\"stylesheet\">"                                                                                                   
#>  [9] "    <link rel=\"stylesheet\" type=\"text/css\" href=\"/flasgger_static/swagger-ui.css\">"                                      
#> [10] "    <link rel=\"icon\" type=\"image/png\" href=\"/static/favicon.ico\" sizes=\"64x64 32x32 16x16\" />"
```

## stream data


```r
(x <- HttpClient$new(url = "https://httpbin.org"))
#> <crul connection> 
#>   url: https://httpbin.org
#>   curl options: 
#>   proxies: 
#>   auth: 
#>   headers: 
#>   progress: FALSE
#>   hooks:
res <- x$get('stream/5', stream = function(x) cat(rawToChar(x)))
#> {"url": "https://httpbin.org/stream/5", "args": {}, "headers": {"Host": "httpbin.org", "Accept": "application/json, text/xml, application/xml, */*", "Accept-Encoding": "gzip, deflate", "User-Agent": "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"}, "origin": "65.197.146.18, 65.197.146.18", "id": 0}
#> {"url": "https://httpbin.org/stream/5", "args": {}, "headers": {"Host": "httpbin.org", "Accept": "application/json, text/xml, application/xml, */*", "Accept-Encoding": "gzip, deflate", "User-Agent": "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"}, "origin": "65.197.146.18, 65.197.146.18", "id": 1}
#> {"url": "https://httpbin.org/stream/5", "args": {}, "headers": {"Host": "httpbin.org", "Accept": "application/json, text/xml, application/xml, */*", "Accept-Encoding": "gzip, deflate", "User-Agent": "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"}, "origin": "65.197.146.18, 65.197.146.18", "id": 2}
#> {"url": "https://httpbin.org/stream/5", "args": {}, "headers": {"Host": "httpbin.org", "Accept": "application/json, text/xml, application/xml, */*", "Accept-Encoding": "gzip, deflate", "User-Agent": "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"}, "origin": "65.197.146.18, 65.197.146.18", "id": 3}
#> {"url": "https://httpbin.org/stream/5", "args": {}, "headers": {"Host": "httpbin.org", "Accept": "application/json, text/xml, application/xml, */*", "Accept-Encoding": "gzip, deflate", "User-Agent": "libcurl/7.54.0 r-curl/3.3 crul/0.7.4"}, "origin": "65.197.146.18, 65.197.146.18", "id": 4}
# when streaming, content is NULL
res$content 
#> NULL
```
