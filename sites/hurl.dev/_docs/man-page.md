---
layout: doc
title: Man Page
section: Getting Started
---
# {{ page.title }}
## NAME {#name}

hurl - run and test HTTP requests.


## SYNOPSIS {#synopsis}

**hurl** [options] [FILE...]


## DESCRIPTION {#description}

**Hurl** is an HTTP client that performs HTTP requests defined in a simple plain text format.

Hurl is very versatile, it enables to chain HTTP requests, capture values from HTTP responses and make asserts.

{% raw %}
```
$ hurl session.hurl
```
{% endraw %}

If no input-files are specified, input is read from stdin.

{% raw %}
```
$ echo GET http://httpbin.org/get | hurl
    {
      "args": {},
      "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip",
        "Content-Length": "0",
        "Host": "httpbin.org",
        "User-Agent": "hurl/0.99.10",
        "X-Amzn-Trace-Id": "Root=1-5eedf4c7-520814d64e2f9249ea44e0f0"
      },
      "origin": "1.2.3.4",
      "url": "http://httpbin.org/get"
    }
```
{% endraw %}


Output goes to stdout by default. For output to a file, use the -o option:

{% raw %}
```
$ hurl -o output input.hurl
```
{% endraw %}



By default, Hurl executes all HTTP requests and outputs the response body of the last HTTP call.



## HURL FILE FORMAT {#hurl-file-format}

The Hurl file format is fully documented in [https://hurl.dev/docs/hurl-file.html](https://hurl.dev/docs/hurl-file.html)

It consists of one or several HTTP requests

{% raw %}
```hurl
GET http:/example.net/endpoint1
GET http:/example.net/endpoint2
```
{% endraw %}


### Capturing values

A value from an HTTP response can be-reused for successive HTTP requests.

A typical example occurs with csrf tokens.

{% raw %}
```hurl
GET https://example.net
HTTP/1.1 200
# Capture the CSRF token value from html body.
[Captures]
csrf_token: xpath "normalize-space(//meta[@name='_csrf_token']/@content)"

# Do the login !
POST https://example.net/login?user=toto&password=1234
X-CSRF-TOKEN: {{csrf_token}}
```
{% endraw %}

### Asserts

The HTTP response defined in the Hurl session are used to make asserts.

At the minimum, the response includes the asserts on the HTTP version and status code.

{% raw %}
```hurl
GET http:/google.com
HTTP/1.1 302
```
{% endraw %}

It can also include asserts on the response headers

{% raw %}
```hurl
GET http:/google.com
HTTP/1.1 302
Location: http://www.google.com
```
{% endraw %}

You can also include explicit asserts combining query and predicate

{% raw %}
```hurl
GET http:/google.com
HTTP/1.1 302
[Asserts]
xpath "//title" equals "301 Moved"
```
{% endraw %}

Thanks to asserts, Hurl can be used as a testing tool to run scenarii.




## OPTIONS {#options}

Options that exist in curl have exactly the same semantic.


### \-\-append {#append}

This option can only be used with [\-\-json](#json). It appends sessions to existing file instead of overwriting it.
This is typically used in a CI pipeline.


### \-\-color {#color}

Colorize Output



### -b, \-\-cookie &lt;file> {#cookie}

Read cookies from file (using the Netscape cookie file format).

Combined with [-c, \-\-cookie-jar](#cookie-jar), you can simulate a cookie storage between successive Hurl runs.


### \-\-compressed {#compressed}

Request a compressed response using one of the algorithms br, gzip, deflate and automatically decompress the content.


### \-\-connect-timeout &lt;seconds> {#connect-timeout}

Maximum time in seconds that you allow Hurl's connection to take.

See also [-m, \-\-max-time](#max-time) option.


### -c, \-\-cookie-jar &lt;file> {#cookie-jar}

Write cookies to FILE after running the session (only for one session).
The file will be written using the Netscape cookie file format.

Combined with [-b, \-\-cookie](#cookie),you can simulate a cookie storage between successive Hurl runs.



### \-\-fail-at-end {#fail-at-end}

Continue executing requests to the end of the Hurl file even when an assert error occurs.
By default, Hurl exits after an assert error in the HTTP response.

Note that this option does not affect the behavior with multiple input Hurl files.

All the input files are executed independently. The result of one file does not affect the execution of the other Hurl files.


### \-\-file-root &lt;dir> {#file-root}

Set root filesystem to import files in Hurl. This is used for both files in multipart form data and request body.
When this is not explicitly defined, the files are relative to the current directory in which Hurl is running.




### -h, \-\-help {#help}

Usage help. This lists all current command line options with a short description.



### \-\-html &lt;dir> {#html}

Generate html report in dir.

If you want to combine results from different Hurl executions in a unique html report, you must also use the options [\-\-json](#json) and [\-\-append](#append).



### -i, \-\-include {#include}

Include the HTTP headers in the output (last entry).


### \-\-interactive {#interactive}

Stop between requests.
This is similar to a break point, You can then continue (Press C) or quit (Press Q).


### \-\-json &lt;file> {#json}

Write full session(s) to a json file. The format is very closed to HAR format.

By default, this file is overwritten by the current run execution.
In order to append sessions to an existing json file, the option [\-\-append](#append) must be used.
This is typically used in a CI pipeline.



### -k, \-\-insecure {#insecure}

This option explicitly allows Hurl to perform "insecure" SSL connections and transfers.



### -L, \-\-location {#location}

Follow redirect.  You can limit the amount of redirects to follow by using the [\-\-max-redirs](#max-redirs) option.


### -m, \-\-max-time &lt;seconds> {#max-time}

Maximum time in seconds that you allow a request/response to take. This is the standard timeout.

See also [\-\-connect-timeout](#connect-timeout) option.


### \-\-max-redirs &lt;num> {#max-redirs}

Set maximum number of redirection-followings allowed
By default, the limit is set to 50 redirections. Set this option to -1 to make it unlimited.


### \-\-no-color {#color}

Do not colorize Output



### \-\-noproxy &lt;no-proxy-list> {#noproxy}

Comma-separated list of hosts which do not use a proxy.
Override value from Environment variable no_proxy.



### \-\-to-entry &lt;entry-number> {#to-entry}

Execute Hurl file to ENTRY_NUMBER (starting at 1).
Ignore the remaining of the file. It is useful for debugging a session.



### -o, \-\-output &lt;file> {#output}

Write output to &lt;file> instead of stdout.



### -x, \-\-proxy [protocol://]host[:port] {#proxy}

Use the specified proxy.

### -u, \-\-user &lt;user:password> {#user}

Add basic Authentication header to each request.


### \-\-variable &lt;name=value> {#variable}

Define variable (name/value) to be used in Hurl templates.
Only string values can be defined.


### \-\-variables-file &lt;file> {#variables-file}

Set properties file in which your define your variables.

Each variable is defined as name=value exactly as with [\-\-variable](#variable) option.

Note that defining a variable twice produces an error.


### -v, \-\-verbose {#verbose}

Turn on verbose output on standard error stream
Useful for debugging.

A line starting with '>' means data sent by Hurl.
A line staring with '&lt;' means data received by Hurl.
A line starting with '*' means additional info provided by Hurl.

If you only want HTTP headers in the output, -i, \-\-include might be the option you're looking for.


### -V, \-\-version {#version}

Prints version information



## ENVIRONMENT {#environment}

Environment variables can only be specified in lowercase.

Using an environment variable to set the proxy has the same effect as using
the [-x, \-\-proxy](#proxy) option.

### http_proxy [protocol://]&lt;host>[:port]

Sets the proxy server to use for HTTP.


### https_proxy [protocol://]&lt;host>[:port]

Sets the proxy server to use for HTTPS.


### all_proxy [protocol://]&lt;host>[:port]

Sets the proxy server to use if no protocol-specific proxy is set.

### no_proxy &lt;comma-separated list of hosts>

list of host names that shouldn't go through any proxy.


## EXIT CODES {#exit-codes}

### 1
Failed to parse command-line options.


### 2
Input File Parsing Error.


### 3
Runtime error (such as failure to connect to host).


### 4
Assert Error.



## WWW {#www}

[https://hurl.dev](https://hurl.dev)


## SEE ALSO {#see-also}

curl(1)  hurlfmt(1)


