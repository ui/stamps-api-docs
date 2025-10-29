************************************
Expiration Journals API
************************************

1. Index
=======================
| URL endpoint: https://stamps.co.id/api/v2/expiration-journals/
| Allowed method: GET
| Requires authentication: Yes


A. Request
-----------------------------

You can get list of expiration journals for certain user by using this parameters:


======================= =========== =======================
Parameter               Required    Description
======================= =========== =======================
user                    Yes         Email address / Member ID indicating customer
minimum_expiration_date No          Minimum expiration date to be displayed.
                                    Will display all user's expiration journals if not defined.
                                    Format: YYYY-MM-DD
======================= =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "user": "customer@stamps.co.id",
       "minimum_expiration_date": "2022-06-25"
    }


Example of API call request using cURL.

.. code-block:: bash

    $ curl --request GET -H "Content-Type: application/json" -H "Expect:" -H "Authorization: <token_type> <token>" 'https://stamps.co.id/api/v2/expiration-journals/?user=customer@stamps.id&minimum_expiration_date=2022-06-25'


B. Response
-----------------------------

In response to this API call, Stamps will reply with list of `expiration_journals`, with following objects:

=================== ==================
Variable            Description
=================== ==================
id                  Unique identifer
stamps_to_expire    Amount of stamps that can be expired in the journal
expiration_date     Date of expiration(YYYY-MM-DD)
=================== ==================

Depending on the request, responses may return these status codes:

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request, usually missing a required parameter
401                 Unauthorized, usually missing or wrong authentication token
403                 Forbidden – You do not have permission for this request
405                 HTTP method not allowed
500, 502, 503, 504  Server errors - something is wrong on Stamps' end
=================== ==============================

Below are an example response on successful API calls.

.. code-block :: bash
    
    {
        "expiration_journals": [
            {
                "id": 284614,
                "stamps_to_expire": 10,
                "expiration_date": "2023-03-30"
            },
            {
                "id": 284615,
                "stamps_to_expire": 10,
                "expiration_date": "2023-03-30"
            },
            {
                "id": 284616,
                "stamps_to_expire": 10,
                "expiration_date": "2023-03-30"
            }
        ]
    }
