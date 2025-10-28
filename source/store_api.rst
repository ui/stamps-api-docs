************************************
Store API
************************************

1. Add or Update Store
=======================================
| URL endpoint: https://stamps.co.id/api/stores/add-or-update
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can add a new store or update an existing store in Stamps using this API.

============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
merchant_id                    Yes         Merchant ID indicated which merchant will the user access
name                           Yes         Store name
display_name                   Yes         Store display name
code                           Yes         Store code
address                        Yes         Address
area                           Yes         Area name
is_active                      Yes         Boolean
============================== =========== ===================================================================

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/stores/add-or-update -i -d '{ "merchant_id": 1, "name": "TEST", "code": "CODE", "display_name": "Test Name", "is_active": true, "address": "New Street", "area": "Area" }'


B. Response
----------------
Please see :ref:`response-codes` for a complete list of API response codes.

Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
store               Various store data
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
      "store": {
        "id": "1"
        "name": "store-1",
        "code": "store-1",
        "display_name": "Store 1",
        "area": "Jakarta",
        "address": "Example street",
        "phone": "",
        "email": "",
        "slug": "test-store",
        "latitude": null,
        "longitude": null,
        "photo_url": "",
        "is_active": true
      }
    }
