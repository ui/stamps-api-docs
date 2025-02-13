************************************
Merchant API
************************************

1. Add Payment Method
=======================================
| URL endpoint: https://stamps.co.id/api/merchants/add-payment-method
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
token                          Yes         Authentication string
merchant_id                    Yes         Merchant ID indicated which merchant will the user access
name                           Yes         Store name
code                           Yes         Store code
eligible_for_points            Yes         Boolean whether transactions using this payment method can get stamps
============================== =========== ===================================================================

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/merchants/add-payment-method -i -d '{ "token": "secret", "merchant_id": 1, "name": "VISA", "code": "VISA", "eligible_for_points": true }'


B. Response
----------------
Please see :ref:`response-codes` for a complete list of API response codes.

Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              `ok` when successful
errors              Errors encountered when parsing
                    data (if any)
=================== ==============================


C. Examples
-----------

On a successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "status": "ok"
    }
