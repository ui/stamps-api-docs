************************************
Promo Code API
************************************


1. Validating a Promo Code
====================================
| URL endpoint: https://stamps.co.id/api/promo-codes/validate
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

==================  =========== =========================
Parameter           Required    Description
==================  =========== =========================
promo_code          Yes         Promo Code's code
store               Yes         Integer indicating store ID
channel             No          Channel of a transaction, see :ref:`channel mapping <Channel Mapping>`
user                No          User identifier
transaction_value   No          Transaction value that will be used for redemption
==================  =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -H "Authorization: <token_type> <token>" 'https://stamps.co.id/api/promo-codes/validate?promo_code=PROMO_CODE&store=123'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
promo_code          An object containing promo code information.
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

On successful balance update:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

    {
        "promo_code": {
            "id": 1,
            "merchant_id": 1,
            "code": "PROMOCODE",
            "name": "Promo Code",
            "description": "",
            "start_date": "2024-05-12",
            "end_date": "2024-06-14",
            "is_active": true,
            "extra_data": {},
            "constraint": {
                "start_time": "10:00",
                "end_time": "18:00",
                "registration_statuses": [],
                "redeemable_in_all_merchants": true,
                "redeemable_in_all_stores": true,
                "max_usage_per_person": 7,
                "max_daily_usage_per_person": 1
            }
        }
    }
