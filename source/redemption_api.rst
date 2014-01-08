************************************
Redemption API
************************************

1. Adding Reward Redemption
===========================

| URL endpoint: https://stamps.co.id/api/redemptions/add
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a redemption by calling the API with these parameters.

=============== ========================================= =========================
Parameter       Required            Description
=============== ========================================= =========================
token           Yes                                       Authentication string
user_email      Yes                                       A string indicating user's
                                                          email address
store           Yes                                       Merchant's store id where
                                                          transaction is initiated
reward          Yes (if reward_by_code is not specified)  A number indicating the
                                                          reward's ID
reward_by_code  Yes (if reward is not specified)          A string of the reward's code
invoice_number  No                                        POS invoice number
type            No                                        Redemption type which by default is
                                                          'reward'. The choices are
                                                          'reward' and 'promotion'
=============== ========================================= =========================

Here's an example of how the API call might look like in JSON format with specified reward

.. code-block :: bash

    {
        "token": "abc",
        "user_email": "customer@stamps.co.id",
        "store": 32,
        "reward": 1
    }

Here's an example of how the API call might look like in JSON format with specified reward_by_code

.. code-block :: bash

    {
        "token": "abc",
        "user_email": "customer@stamps.co.id",
        "store": 32,
        "reward_by_code": "100"
    }

Example of API call request using cURL with specified reward

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user_email": "customer@stamps.co.id", "store": 32, "reward": 12}' https://stamps.co.id/api/redemptions/add

Example of API call request using cURL with specified reward_by_code

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user_email": "customer@stamps.co.id", "store": 32, "reward_by_code": "100"}' https://stamps.co.id/api/redemptions/add

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

2. Adding Voucher Redemption
============================

| URL endpoint: https://stamps.co.id/api/redemptions/add-voucher
| Allowed method: POST
| Requires authentication: Yes


A. Parameters
-------------

You can initiate a voucher redemption by calling the API with these parameters.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating customer's email address
store       Yes         Merchant's store id where redemption is initiated
voucher     Yes         A number indicating the voucher's id
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "abc",
        "user": "customer@stamps.co.id",
        "store": 32,
        "voucher": 1
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user": "customer@stamps.co.id", "store": 32, "voucher": 12}' https://stamps.co.id/api/redemptions/add-voucher


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          An object containing various redemption information
                    Contains redemption id and name of voucher redeemed
customer            An object containing customer information after successful
                    redemption. Contains id and remaining Stamps.
detail              Description of error (if any)
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
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
        "voucher": "Kaya Toast Voucher",
        "id": 1
      }
    }

3. Cancel Existing Redemption
==============================

| URL endpoint: https://stamps.co.id/api/redemptions/cancel
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can cancel a redemption by calling the API with these parameters.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
id          Yes         Redemption ID
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "token": "secret",
        "id": 1
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "secret", "id": 1 }' https://stamps.co.id/api/redemptions/cancel

B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          Redemption information which is
                    successfully canceled.
                    Contains id and status
customer            Customer information after successful
                    redemption. Contains id and stamps_remaining.
errors              Errors encountered when processing request (if any)
=================== ==============================

C. Response Headers
-------------------

Depending on the request, responses may return these status codes:

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a required parameter
401                 Unauthorized – Often missing or wrong authentication token
403                 Forbidden – You do not have permission for this request
404                 Cannot find redemption of the requested redemption id
405                 HTTP method not allowed
500, 502, 503, 504  Server Errors - something is wrong on Stamps' end
=================== ==============================

D. Example Response
-------------------

Below are a few examples responses on successful API calls.


If redemption is successfully canceled:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "redemption": {
        "id": 1,
        "status": "Canceled"
      },
      "customer": {
        "status": "Blue",
        "id": 6,
        "stamps_remaining": 60
      }
    }
