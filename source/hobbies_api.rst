************************************
Hobbies API
************************************

1. Set Hobbies
===============
| URL endpoint: https://stamps.co.id/api/hobbies/set
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to set customer hobbies.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
hobbies       Yes         List of hobbies code
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/hobbies/set -i -d '{ "token": "secret", "user": 123, "hobbies": ["stuff", "things"]}'


B. Response Data
----------------
Stamps responds to this API call with the following data:

=================== ==============================
Variable            Description
=================== ==============================
hobbies             Updated customer hobbies
=================== ==============================


C. Response Codes
-----------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized – Often missing or
                    wrong authentication token
403                 Forbidden – You do not have
                    permission for this request
405                 HTTP method not allowed - The
                    requested resources cannot be called with the specified HTTP method
500, 502, 503, 504  Server Errors - something is
                    wrong on Stamps' end
=================== ==============================


D. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "hobbies": [
            {
                'id': 1,
                'code': 'stuff',
                'name': 'Stuff',
            },
            {
                'id': 2,
                'code': 'things',
                'name': 'Things',
            }
        ],
    }
