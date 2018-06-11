************************************
Redemption API
************************************

1. Adding Reward Redemption
======================

| URL endpoint: https://stamps.co.id/api/v2/redemptions/redeem-reward
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a reward redemption by calling the API with these parameters.

=============== ========= =========================
Parameter       Required  Description
=============== ========= =========================
token           Yes       Authentication string
user            Yes       A string indicating customer's email or Member ID
reward          Yes       A number indicating the reward's ID
store           Yes       Merchant's store id where redemption is initiated
invoice_number  No        POS invoice number
=============== ========= =========================

Here's an example of how the API call might look like in JSON format with specified reward

.. code-block :: bash

    {
        "token": "abc",
        "user": "customer@stamps.co.id",
        "store": 32,
        "reward": 1
        "invoice_number": "POS-1020123"
    }

Example of API call request using cURL with specified reward

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user": "customer@stamps.co.id", "store": 32, "reward": 12}' https://stamps.co.id/api/v2/redemptions/redeem-reward


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          Redemption information which is
                    successfully created.
                    Contains id, reward, and stamps_used
membership          Membership information after successful
                    redemption. Contains membership id and stamps_remaining.
reward              Reward information
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
        "redemption": {
            "id": 3,
            "reward": "Kaya Bun",
            "stamps_used": 2,
            "extra_data": {
                "discount": "10%"
            }
        },
        "membership": {
            "tags": [],
            "status": 100,
            "stamps": 250,
            "balance": 0,
            "referral_code": "9121682",
            "start_date": "2016-07-25",
            "created": "2016-07-25"
        },
        "reward": {
            "id": 1,
            "name": "Kaya Bun",
            "stamps_to_redeem": 2,
            "extra_data": {},
            "code": "MI0017",
            "type": "reward"
        }
    }


E. Legacy Endpoint
------------------
Legacy endpoint's documentation is available at `Legacy redemption API <http://docs.stamps.co.id/en/latest/legacy_redemption_api.html>`_

2. Adding Voucher Redemption
======================

| URL endpoint: https://stamps.co.id/api/v2/redemptions/redeem-voucher
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a reward redemption by calling the API with these parameters.

=============== ========= =========================
Parameter       Required  Description
=============== ========= =========================
token           Yes       Authentication string
user            Yes       A string indicating customer's email or Member ID
voucher         Yes       An integer indicating the voucher's ID
store           Yes       Merchant's store id where redemption is initiated
invoice_number  No        POS invoice number
=============== ========= =========================

Here's an example of how the API call might look like in JSON format with specified voucher.

.. code-block :: bash

    {
        "token": "abc",
        "user": "customer@stamps.co.id",
        "store": 32,
        "voucher": 1
        "invoice_number": "POS-1020123"
    }

API call example:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user": "customer@stamps.co.id", "store": 32, "voucher": 12}' https://stamps.co.id/api/v2/redemptions/redeem-voucher


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          Redemption information which is
                    successfully created.
                    Contains id, reward, and stamps_used
membership          Customer information after successful
                    redemption. Contains id and stamps_remaining.
voucher             Voucher used in redemption
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
       "redemption": {
            "id": 2,
            "reward": "Discount Rp 100,000",
            "stamps_used": 0,
            "extra_data": {
                "discount": "10%"
            }
        },
        "membership": {
            "tags": [],
            "status": 100,
            "stamps": 250,
            "balance": 0,
            "referral_code": "9121682",
            "start_date": "2016-07-25",
            "created": "2016-07-25"
        },
        "voucher": {
            "id": 4,
            "name": "Discount Rp 100,000",
            "code": "PZ633ECV",
            "type": "voucher"
        }
    }

E. Legacy Endpoint
------------------
Legacy endpoint's documentation is available at `Legacy redemption API <http://docs.stamps.co.id/en/latest/legacy_redemption_api.html>`_

3. Adding Redemption by Voucher Code
======================

| URL endpoint: https://stamps.co.id/api/redemptions/by-voucher-code
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a reward redemption by calling the API with these parameters.

=============== ========= =========================
Parameter       Required  Description
=============== ========= =========================
token           Yes       Authentication string
identifier      Yes       A string indicating customer's email or phone
voucher_code    Yes       An string that indicating a voucher code
voucher_template Yes      An integer indicating the voucher's ID
store           Yes       Merchant's store id where redemption is initiated
=============== ========= =========================

Here's an example of how the API call might look like in JSON format with specified voucher.

.. code-block :: bash

    {
        "token": "abc",
        "identifier": "customer@stamps.co.id",
        "voucher_code": "ABCD100k",
        "voucher_template": 12,
        "store": 32
    }

API call example:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "abc", "user": "customer@stamps.co.id", "voucher_code": "ABCD100k", "voucher_template": 12, "store": 32}' https://stamps.co.id/api/redemptions/by-voucher-code


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
                    redemption. Contains id, membership status, and stamps_remaining.
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
        "redemption": {
        "id": 199061,
        "status": "Created"
        },
        "customer": {
        "id": 401791,
        "status": "Blue",
        "stamps_remaining": 0
        }
    }

E. Legacy Endpoint
------------------
Legacy endpoint's documentation is available at `Legacy redemption API <http://docs.stamps.co.id/en/latest/legacy_redemption_api.html>`_

4. Canceling a Redemption
=========================

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
