************************************
Voucher API
************************************

1. Issuing a voucher to user
====================================
| URL endpoint: https://stamps.co.id/api/vouchers/issue/
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can add issue a voucher by calling the API with these parameters.

===========      =========== =========================
Parameter        Required    Description
===========      =========== =========================
token            Yes         Authentication string
user             Yes         A string indicating user's email address or member ID
voucher_template Yes         Integer indicating the voucher template ID
start_date       Yes         Date string to indicate voucher's valid start date
                             (e.g. 2013-02-15)
quantity         Yes         Integer indicating voucher quantity to be given
notes            No          Note tied to the voucher
===========      =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "user": "customer@stamps.co.id",
        "voucher_template": 1,
        "start-date": "2013-02-15",
        "quantity": 1,
        "notes": "Special voucher for e-magazine readers"
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "user": "customer@stamps.co.id", "voucher_template": 1, "start_date": "2013-02-15", "quantity": 1, "notes": "Special voucher for e-magazine readers"}' https://stamps.co.id/api/vouchers/issue/


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

