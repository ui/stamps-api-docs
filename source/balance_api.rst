************************************
Customer Balance API
************************************

1. Add or Reduce Balance
=======================================
| URL endpoint: https://stamps.co.id/api/balances/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can initiate a balance change by calling the API with these parameters.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user		Yes         A string indicating user's email address
amount		Yes			A number indicating the balance change
						(to reduce balance, use negative number)
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "user": "customer@stamps.co.id",
        "amount": 1000,
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "amount": 1000}' https://stamps.co.id/api/balances/change


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            An object containing customer information after successful
                    balance update. Contains id, current balance and membership status.
errors              Errors encountered when processing request (if any)
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
      "customer": {
        "id": 6,
        "balance": 1000,
        "status": "Blue"
      }
    }
