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


2. Deduct customer stamps
=======================================
| URL endpoint: https://stamps.co.id/api/memberships/deduct-stamps
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can deduct customer's stamps amount with this API Call.

==================== =========== =========================
Parameter            Required    Description
==================== =========== =========================
token                Yes         Authentication string
user                 Yes         A string indicating customer's email or Member ID
stamps               Yes         Integer indicating amount of stamps to be deducted from customer
notes                Yes         Notes that explain why stamps are deducted
==================== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/memberships/deduct-stamps -i -d '{ "token": "secret", "user": "customer@stamps.co.id", "stamps": 10, "notes": "Test deduct"}'



Payload example in JSON:

.. code-block :: bash

    {
        "token": "secret",
        "user": 1865,
        "stamps": 1,
        "notes": "test"
    }


B. Response Data
----------------
Stamps response to this API call with the following data (in JSON):

===================================== ==============================
Variable                              Description
===================================== ==============================
membership                            Membership information.
stamps_deduction.id                   Deduction ID
stamps_deduction.stamps               How many stamps that was successfully deducted
stamps_deduction.notes                Notes about the deduction
stamps_deduction.created              Created time of deduction in GMT timezone
stamps_deduction.created_timestamp    Created time of deduction in UNIX timestamp format
detail                                Description of error (if any)
errors                                Errors encountered when parsing
                                      data (if any)
===================================== ==============================


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
        "membership": {
            "id": 1864,
            "group_id": 1,
            "status": 100,
            "status_text": "Blue",
            "stamps": 7,
            "balance": 0,
            "referral_code": "JJ3X1",
            "is_blocked": false,
            "created": "2022-03-31"
        },
        "stamps_deduction": {
            "id": 9,
            "stamps": 1,
            "notes": "test",
            "created": "2022-07-13T08:41:10+00:00",
            "created_timestamp": 1657701670,
            "status": 1
        }
    }

When some fields don't validate:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "detail": "notes: This field is required.",
        "error_message": "notes: This field is required.",
        "error_code": "required",
        "errors": {
            "notes": "This field is required."
        }
    }
