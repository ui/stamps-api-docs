************************************
Redemption API (Legacy)
************************************

1. Adding a Redemption
======================

| URL endpoint: https://stamps.co.id/api/redemptions/add
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a redemption by calling the API with these parameters.

=============== ========= =========================
Parameter       Required  Description
=============== ========= =========================
user            Yes       A string indicating customer's email or Member ID
store           Yes       Merchant's store id where redemption is initiated
reward          Yes       A number indicating the reward's ID
invoice_number  No        POS invoice number
type            No        Choices are "reward" or "voucher".
                          Defaults to "reward".
=============== ========= =========================

Here's an example of how the API call might look like in JSON format with specified reward

.. code-block :: bash

    {
        "user": "customer@stamps.co.id",
        "store": 32,
        "reward": 1
    }

Here's an example of how the API call might look like in JSON format with specified reward_by_code

.. code-block :: bash

    {
        "user": "customer@stamps.co.id",
        "store": 32,
        "reward_by_code": "100"
    }

Example of API call request using cURL with specified reward

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" -d '{ "user": "customer@stamps.co.id", "store": 32, "reward": 12}' https://stamps.co.id/api/redemptions/add

Example of API call request using cURL with specified reward_by_code

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" -d '{ "user": "customer@stamps.co.id", "store": 32, "reward_by_code": "100"}' https://stamps.co.id/api/redemptions/add

B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          Redemption information which is
                    successfully created.
                    Contains id, reward, and stamps_used
customer            Customer information after successful
                    redemption. Contains id and stamps_remaining.
vouchers            Voucher information if redemption is generating one,
                    This field do not exist if redemption is not creating voucher.
errors              Errors encountered when processing request (if any)
=================== ==============================

C. Response Headers
-------------------

Depending on the request, responses may return these status codes:

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
405                 HTTP method not allowed
500, 502, 503, 504  Server Errors - something is wrong on Stamps' end
=================== ==============================

D. Example Response
-------------------

On successful redemption:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "customer": {
        "id": 6,
        "stamps_remaining": 60
      },
      "redemption": {
        "reward": "Free Scoop of Ice Cream",
        "id": 1,
        "stamps_used": 10
      }
    }


On successful redemption that generate voucher:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "customer": {
        "id": 6,
        "stamps_remaining": 60
      },
      "redemption": {
        "reward": "Free Scoop of Ice Cream voucher",
        "id": 1,
        "stamps_used": 10
      },
      "voucher": {
          "id": 2034,
          "name": "Free Scoop of Ice Cream voucher",
          "type": "Voucher #2034",
          "quantity": 1,
          "image_url": "http://foo.com",
          "expires_on": "5-12-2013 23:59"
      }
    }


