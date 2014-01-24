************************************
Balance API
************************************

1. Crediting to a Customer's Balance
====================================
| URL endpoint: https://stamps.co.id/api/balances/add
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
amount      Yes         A positive number indicating the amount to be added to customer's balance
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

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "amount": 1000}' https://stamps.co.id/api/balances/add


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            An object containing customer information after successful addition
                    to the balance. Contains id, current balance and membership status.
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

2. Debiting from a Customer's Balance
=====================================
| URL endpoint: https://stamps.co.id/api/balances/deduct
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can deduct an amount from a balance by calling the API with these parameters.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating user's email address or member ID
amount      Yes         A positive number indicating amount to be deducted from customer's balance
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "user": "customer@stamps.co.id",
        "amount": 100,
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "amount": 100}' https://stamps.co.id/api/balances/deduct


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            An object containing customer information after successful deduction
                    from the balance. Contains id, current balance and membership status.
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
        "balance": 900,
        "status": "Blue"
      }
    }
