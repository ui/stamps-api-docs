************************************
Stamps API
************************************

1. Add customer stamps
=======================================
| URL endpoint: https://stamps.co.id/api/stamps/add
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can add customer's stamps amount with this API Call.

==================== =========== =========================
Parameter            Required    Description
==================== =========== =========================
token                Yes         Authentication string
user                 Yes         A string indicating customer's email or Member ID
store                Yes         Integer indicating store ID
number_of_stamps     Yes         Integer indicating amount of stamps to be added to customer
==================== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/stamps/add -i -d '{ "token": "secret", "user": "customer@stamps.co.id", "store": 2, "number_of_stamps": 10}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Customer information after successful query. Contains id, stamps, and status.
awards              Award information for this stamps addition. Contains id, number_of_stamps
detail              Description of error (if any)
errors              Errors encountered when parsing
                    data (if any)
=================== ==============================


C. Response Headers
-------------------

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

On a successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "customer": {
          "id": 114807,
          "stamps": 18,
          "status": "Blue"
      },
      "award": {
          "id": 1010,
          "number_of_stamps": 10
      }
    }


