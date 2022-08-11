************************************
User API
************************************

1. Validate User Password
====================================
| URL endpoint: https://stamps.co.id/api/auth/check-password
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can add an amount to a balance by calling the API with these parameters.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating user's email address or member ID
password    Yes         User's password
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "user": "customer@stamps.co.id",
        "password": "secret123"
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "password": "secret123"}' https://stamps.co.id/api/auth/check-password


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              status
=================== ==============================


C. Example Response
-------------------

On successful balance update:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "ok"
    }
