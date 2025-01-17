---
layout: home
title: Hurl - Run and Test HTTP Requests
section: Home
---

<div class="home-logo">
    <img class="light-img" src="{{ '/assets/img/logo-light.svg' | prepend:site.baseurl }}" width="264px" />
    <img class="dark-img" src="{{ '/assets/img/logo-dark.svg' | prepend:site.baseurl }}" width="264px" />
</div>



# What's Hurl?

Hurl is a command line tool that runs HTTP requests defined in a simple plain text format.

It can perform requests, capture values and evaluate queries on headers and body response. Hurl is very versatile:
it can be used for both fetching data and testing HTTP sessions.

{% raw %}
```hurl
# Get home:
GET https://example.net

HTTP/1.1 200
[Captures]
csrf_token: xpath "string(//meta[@name='_csrf_token']/@content)"

# Do login!
POST https://example.net/login?user=toto&password=1234
X-CSRF-TOKEN: {{csrf_token}}

HTTP/1.1 302
```
{% endraw %}

Chaining multiple requests is easy:

```hurl
GET https://api.example.net/health
GET https://api.example.net/step1
GET https://api.example.net/step2
GET https://api.example.net/step3
```

# Also an HTTP Test Tool

Hurl can run HTTP requests but can also be used to test HTTP responses.
Different types of queries and predicates are supported, from [XPath](https://en.wikipedia.org/wiki/XPath)
and [JSONPath](https://goessner.net/articles/JsonPath/) on body response, to assert on status code and response headers.

```hurl
GET https://example.net

HTTP/1.1 200
[Asserts]
xpath "normalize-space(//head/title)" equals "Hello world!"
```

It is well adapted for REST/json apis

```hurl
POST https://api.example.net/tests
{
    "id": "456",
    "evaluate": true
}

HTTP/1.1 200
[Asserts]
jsonpath "$.status" equals "RUNNING"      # Check the status code
jsonpath "$.tests" countEquals 25         # Check the number of items

```

and even SOAP apis

```hurl
POST https://example.net/InStock
Content-Type: application/soap+xml; charset=utf-8
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="http://www.example.org">
  <soap:Header></soap:Header>
  <soap:Body>
    <m:GetStockPrice>
      <m:StockName>GOOG</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>

HTTP/1.1 200
```

Hurl can also be used to test HTTP endpoints performances:

```hurl
GET http://api.example.org/v1/pets

HTTP/1.0 200
[Asserts]
duration lessThan 1000  # Duration in ms
```

# Why Hurl?

<ul class="showcase-container">
 <li class="showcase-item"><h2 class="showcase-item-title">Text Format</h2>For both devops and developers</li>
 <li class="showcase-item"><h2 class="showcase-item-title">Fast CLI</h2>A command line for local dev and continuous 
integration</li>
 <li class="showcase-item"><h2 class="showcase-item-title">Single Binary</h2>Easy to install, with no runtime 
required</li>
</ul>

# Powered by curl

Hurl is a lightweight binary written in [Rust](https://www.rust-lang.org). Under the hood, Hurl HTTP engine is
 powered by [libcurl](https://curl.haxx.se/libcurl/), one of the most powerful and reliable file transfer library.
 With its text file format, Hurl adds syntactic sugar to run and tests HTTP requests, but it's still the [curl](https://curl.se)
 that we love.



# Installation

See the [the installation section]({% link _docs/installation.md %}).

# Feedbacks

Hurl file format and runners are still in beta, any [feedback, suggestion, bugs or improvements](https://github.com/Orange-OpenSource/hurl/issues)
 are welcome.

```hurl
POST https://hurl.dev/api/feedback
{
  "name": "John Doe",
  "feedback": "Hurl is awesome !"
}
HTTP/1.1 200
```

# Resources

[License]({% link _docs/license.md %})

[Documentation]({% link _docs/man-page.md %})

[GitHub](https://github.com/Orange-OpenSource/hurl)

