### Simple Retrieval ###

```
import httplib2
h = httplib2.Http(".cache")
resp, content = h.request("http://example.org/", "GET")
```

The 'content' is the content retrieved from the URL. The content is already decompressed or unzipped if necessary. The 'resp' contains all the response headers.

Python 3 makes a distinction between [bytes and strings](http://diveintopython3.org/strings.html). In httplib2, the response headers are strings, but the content is bytes. If you want to turn the content into a string, you need to [determine the character encoding](http://www.w3.org/International/O-charset), then explicitly convert it to a string. The exact algorithm for doing this depends on the media type; httplib2 can not help you determine the character encoding.

Once you determine the character encoding, the rest is easy. For example, if you determine that the encoding is UTF-8, you would say:

```
str_content = content.decode('utf-8')
```

### Authentication ###

To PUT some content to a server that uses SSL and Basic authentication:
```
import httplib2
h = httplib2.Http(".cache")
h.add_credentials('name', 'password')
resp, content = h.request("https://example.org/chap/2", 
    "PUT", body="This is text", 
    headers={'content-type':'text/plain'} )
```

### Cache-Control ###

Use the `Cache-Control:` header to control how the caching operates.

```
import httplib2
h = httplib2.Http(".cache")
resp, content = h.request("http://bitworking.org/")
 ...
resp, content = h.request("http://bitworking.org/", 
    headers={'cache-control':'no-cache'})
```

The first request will be cached and since this is a request to bitworking.org it will be set to be cached for two hours, because that is how I have my server configured. Any subsequent GET to that URI will return the value from the on-disk cache and no request will be made to the server. You can use the `Cache-Control:` header to change the caches behavior and in this example the second request adds the `Cache-Control:` header with a value of 'no-cache' which tells the library that the cached copy must not be used when handling this request.

### Forms ###

Below is an example of using `httplib2` to submit a form. Note that we have to use the `urlencode()` function from `urllib.parse` to encode the data before using it as the POST body.
```
>>> from httplib2 import Http
>>> from urllib.parse import urlencode
>>> h = Http()
>>> data = {"name": "Joe", "comment": "A test comment"}
>>> resp, content = h.request("http://bitworking.org/news/223/Meet-Ares", "POST", urlencode(data))
>>> resp
{'status': '200', 'transfer-encoding': 'chunked', 'vary': 'Accept-Encoding,User-Agent',
 'server': 'Apache', 'connection': 'close', 'date': 'Tue, 31 Jul 2007 15:29:52 GMT', 
 'content-type': 'text/html'}
```

### Cookies ###

When automating something, you often need to "login" to maintain some sort of session/state with the server.  Sometimes this is achieved with form-based authentication and cookies. You post a form to the server, and it responds with a cookie in the incoming HTTP header.  You need to pass this cookie back to the server in subsequent requests to maintain state or to keep a session alive.

Here is an example of how to deal with cookies when doing your HTTP Post.

First, lets import the modules we will use:
```
import urllib.parse
import httplib2
```

Now, lets define the data we will need. In this case, we are doing a form post with 2 fields representing a username and a password.

```
url = 'http://www.example.com/login'   
body = {'USERNAME': 'foo', 'PASSWORD': 'bar'}
headers = {'Content-type': 'application/x-www-form-urlencoded'}
```
Now we can send the HTTP request:
```
http = httplib2.Http()
response, content = http.request(url, 'POST', headers=headers, body=urllib.parse.urlencode(body))
```

At this point, our "response" variable contains a dictionary of HTTP header fields that were returned by the server. If a cookie was returned, you would see a "set-cookie" field containing the cookie value. We want to take this value and put it into the outgoing HTTP header for our subsequent requests:

```
headers['Cookie'] = response['set-cookie']
```

Now we can send a request using this header and it will contain the cookie, so the server can recognize us.

So... here is the whole thing in a script. We login to a site and then make another request using the cookie we received:

```
#!/usr/bin/python3

import urllib.parse
import httplib2

http = httplib2.Http()

url = 'http://www.example.com/login'   
body = {'USERNAME': 'foo', 'PASSWORD': 'bar'}
headers = {'Content-type': 'application/x-www-form-urlencoded'}
response, content = http.request(url, 'POST', headers=headers, body=urllib.parse.urlencode(body))

headers = {'Cookie': response['set-cookie']}

url = 'http://www.example.com/home'   
response, content = http.request(url, 'GET', headers=headers)
```

### Proxies ###

Proxy support is unavailable until the third-party socks module is ported to Python 3.