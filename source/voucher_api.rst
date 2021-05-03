************************************
Voucher API
************************************

1. Issuing a voucher to user
====================================
| URL endpoint: https://stamps.co.id/api/vouchers/issue
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can add issue a voucher by calling the API with these parameters.

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
token            Yes         Authentication string
user             Yes         A string indicating user's email address or member ID
voucher_template Yes         Integer indicating the voucher template ID
start_date       Yes         Date string to indicate voucher's valid start date (e.g. 2013-02-15)
quantity         Yes         Integer indicating voucher quantity to be given
notes            No          Note tied to the voucher
================ =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "user": "customer@stamps.co.id",
        "voucher_template": 1,
        "start_date": "2013-02-15",
        "quantity": 1,
        "notes": "Special voucher for e-magazine readers"
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "voucher_template": 1, "start_date": "2013-02-15", "quantity": 1, "notes": "Special voucher for e-magazine readers"}' https://stamps.co.id/api/vouchers/issue


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            An object containing customer information after successful request.
                    Contains id and email.
voucher             An object containing the information on the voucher created with the request.
                    Contains id, start_date, end_date, quantity, and notes.
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
      "voucher": {
        "id": 123054,
        "start_date": "2014-11-20",
        "end_date": "2014-11-30",
        "quantity": 1,
        "notes": "Special voucher for e-magazine readers"
      },
      "customer": {
        "id": 6,
        "email": "customer@stamps.co.id"
      }
    }


2. Validating a Voucher
====================================
| URL endpoint: https://stamps.co.id/api/vouchers/validate
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can add issue a voucher by calling the API with these parameters.

============     =========== =========================
Parameter        Required    Description
============     =========== =========================
token            Yes         Authentication token in string
voucher_code     Yes         A string indicating voucher code
merchant         Yes         Integer indicating the voucher template ID
store            Yes         Integer indicating store ID to be queried for reward
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/vouchers/validate?token=123&merchant=123&voucher_code=VC-ABC&store=123'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                An object containing customer information.
voucher             An object voucher information.
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
      "is_redeemable": true,
      "user": {
          "email": "foo@bar.com",
          "name": "Alice",
          "phone": "+628123123123"
      },
      "voucher": {
          "extra_data": {
              "discount": 1000,
          },
          "id": 123,
          "name": "Rp. 100,000 Discount",
          "start_date": "2021-04-26",
          "end_date": "2021-05-24",
          "validity": "Dynamic"
      }
  }
  
