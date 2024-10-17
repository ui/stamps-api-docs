************************************
Cards API
************************************

1. Create Card
===============
| URL endpoint: https://stamps.co.id/api/cards/create
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to create a new card.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
token         Yes         Authentication string
number        Yes         Card number
store         No          Integer indiciating store where the card is activated
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/cards/create -i -d '{ "token": "secret", "number": "ABC123456", "store": 1}'


B. Response
----------------
Please see :ref:`response-codes` for a complete list of API response codes.

On a successful API call, Stamps will respond with the following data:

=================== ==============================
Variable            Description
=================== ==============================
number              Created card number
=================== ==============================

C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "number": "ABC123456",
        "is_active": true
    }


2. Assign Card
===============
| URL endpoint: https://stamps.co.id/api/v2/cards/assign-card
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to assign a card to a user.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
token         Yes         Authentication string
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
store         Yes         Integer indiciating store where the card is activated
number        Yes         Card number
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/cards/assign-card -i -d '{ "token": "secret", "user": "1", "store": "1", "number": "ABC123456"}'


B. Response
----------------
Please see :ref:`response-codes` for a complete list of API response codes.

On a successful API call, Stamps will respond with the following data:

=================== ==============================
Variable            Description
=================== ==============================
card                Various card data
=================== ==============================

C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "card": {
            "id": 1,
            "number": "ABC123456",
            "is_active": true,
            "activated_time": "2024-09-13 10:00:00"
        }
    }
