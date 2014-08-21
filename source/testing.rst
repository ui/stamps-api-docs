**************************
Testing and Error Handling
**************************

More often that not, testing your program's behavior error handling behavior is
a tricky thing to do. To this end, we provide a few endpoints to make your lives a bit easier.


1. Account Verification
=======================
| URL endpoint: https://stamps.co.id/api/ping
| Allowed method: GET

This endpoint accepts a single ``token`` argument. Here's an example:

.. code-block :: bash

    $ curl "https://stamps.co.id/api/ping?token=abc"

If the right token is supplied, you'll receive a response similar to this:

.. code-block :: bash

    HTTP/1.0 200 OK
    Allow: POST, OPTIONS
    Content-Type: application/json
    Date: Thu, 21 Aug 2014 01:44:18 GMT
    Server: WSGIServer/0.1 Python/2.7.6

    {
        "pong": {
            "merchant": "My Merchant", 
            "merchant_id": 1
        }
    }


2. Error 500
============
| URL endpoint: https://stamps.co.id/api/500
| Allowed method: GET & POST

This endpoint always returns a 500 error. For example:

.. code-block :: bash

    $ curl "https://stamps.co.id/api/500"

Example response:

.. code-block :: bash

    HTTP/1.0 500 INTERNAL SERVER ERROR
    Content-Type: text/html; charset=utf-8
    Date: Thu, 21 Aug 2014 01:48:49 GMT
    Server: WSGIServer/0.1 Python/2.7.6

    Server error - 500


3. Error 400
============
| URL endpoint: https://stamps.co.id/api/400
| Allowed method: GET & POST

This endpoint always returns a response with 400 error (Bad request). For example:

.. code-block :: bash

    $ curl "https://stamps.co.id/api/400"

Example response:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Content-Type: text/html; charset=utf-8
    Date: Thu, 21 Aug 2014 01:50:39 GMT
    Server: WSGIServer/0.1 Python/2.7.6

    Bad request - 400
