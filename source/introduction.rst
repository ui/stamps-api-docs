************
Introduction
************

To quickly get you started API, here are a few things you should know about Stamps' API.

JSON
----

All POST requests to Stamps' API must be JSON encoded, having "application/json" as Content-Type. JSON is a language independent, standards based
(`RFC 4627 <http://tools.ietf.org/html/rfc4627>`_) data interchange format that's
supported in practically every modern programming language.


If you're not yet familiar with JSON, `Wikipedia <http://en.wikipedia.org/wiki/JSON>`_
has a nice primer on the data interchange format.

Making a HTTP Request
---------------------

Here's an example on how to specify an extra content type header in request using the widely supported cURL - it's freely available on Mac, Linux and Windows. You can get cURL from http://curl.haxx.se/

.. code-block :: bash

    > $ curl –X POST –H "Content-Type: application/json"

Testing tip: we recommend the use of `httpbin <http://httpbin.org/>`_ for testing purposes.
It's a simple service that simply echoes back your HTTP request so you can easily
debug whether your application is sending the right data. For example:

.. code-block :: bash

    $ curl http://httpbin.org/get
    {
       "args": {},
       "headers": {
          "Accept": "*/*",
          "Connection": "close",
          "Content-Length": "",
          "Content-Type": "",
          "Host": "httpbin.org",
          "User-Agent": "curl/7.19.7 (universal-apple-darwin10.0) libcurl/7.19.7 OpenSSL/0.9.8l zlib/1.2.3"
       },
       "origin": "24.127.96.129",
       "url": "http://httpbin.org/get"
    }
