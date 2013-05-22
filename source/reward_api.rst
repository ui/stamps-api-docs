************************************
Reward API
************************************

1. Querying for Available Rewards
=======================================
| URL endpoint: https://stamps.co.id/api/rewards/
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for all available rewards on stamps with optional checking to user's capability to redeem the rewards.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user_email  No          A string indicating user's
                        email address to be queried
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/rewards/?token=abc&user_email=customer@stamps.co.id'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
rewards             List of rewards available for redemption.
                    Contains id, name, stamps_required, image_url, and redeemable(If user_email is provided)
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
      "rewards": [
        {
          "stamps_to_redeem": 20,
          "image_url": "http://foo.com",
          "id": 6,
          "redeemable": true,
          "name": "Mee Goreng"
        },
        {
          "stamps_to_redeem": 60,
          "image_url": "http://foo.com",
          "id": 5,
          "redeemable": true,
          "name": "Curry Chicken"
        },
        {
          "stamps_to_redeem": 120,
          "image_url": "http://foo.com",
          "id": 8,
          "redeemable": false,
          "name": "Nasi Lemak"
        },
        {
          "stamps_to_redeem": 10,
          "image_url": "http://foo.com",
          "id": 7,
          "redeemable": false,
          "name": "Nasi Lemak"
        }
      ]
    }


API call with missing parameters:


.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"reward": "This field is required"}]}


If missing or wrong authentication token:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Authentication credentials were not provided."}

