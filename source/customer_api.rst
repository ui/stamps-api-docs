************************************
Membership API
************************************

1. Getting Member Data
=======================================
| URL endpoint: https://stamps.co.id/api/v2/memberships/details
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for a customer's data on Stamps .

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
merchant    Yes         Integer indicating merchant ID
verbose     No          Boolean on whether to include membership's upgrade requirements
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    # Please note that for cURL command you need to escape special characters
    $ curl 'https://stamps.co.id/api/v2/memberships/details?token=abc&user=customer@stamps.co.id&merchant=14'


B. Response Data
----------------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
memberships         Membership related information
users               User related information
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
      "membership": {
        "tags": [],
        "status": 100,
        "status_text": "Blue",
        "stamps": 401,
        "balance": 150000,
        "is_blocked": false,
        "referral_code": "ABCDE",
        "start_date": "2014-08-08",
        "created": "2014-08-08",
        "extra_data": {},
        "to_upgrade": {
          "spending_requirement": 590000,
          "deadline": "2022-12-31"
        }
      },
      "user": {
        "member_ids": [],
        "id": "8120",
        "name": "Customer",
        "gender": "male",
        "address": "",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "phone": "+6281314811365",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1990-11-26",
        "postal_code": "10310",
        "protected_redemption": false,
        "religion": 1,
        "marital_status": 1,
        "wedding_date": null
      }
    }


API call with missing parameters:


.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "errors": {
        "__all__": "User not found"
      },
      "error_message": "User not found",
      "error_code": "invalid_data",
      "detail": "__all__: User not found"
    }


If missing or wrong authentication token:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Authentication credentials were not provided."}


E. Legacy API
-------------

Legacy endpoint's documentation is available at `Legacy Membership API <http://docs.stamps.co.id/en/latest/legacy_customer_api.html>`_



2. Member Suggestions
=====================
| URL endpoint: https://stamps.co.id/api/memberships/suggestions
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

Manual inputs are time consuming and prone to errors. Member entry interfaces
can be made easier to use by offering autocompletions. Given a sequence of
characters, this API returns a list of possible member matches.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
query       Yes         A string indicating query
                        to be processed for the suggestions API
merchant    Yes         Integer indicating merchant ID
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/memberships/suggestions?token=abc&query=steve&merchant=14'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
suggestions         List of user suggestions.
                    Contains id, name, stamps, email, membership
                    and other customer data similar to those
                    returned by member details API in section 1.
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "suggestions": [
        {
          "membership": "Gold",
          "email": "alice@stamps.co.id",
          "stamps": 100,
          "id": 12,
          "name": "Customer Gold",
          "phone": "+6281123123",
          "address": "Baker Street 221B",
          "gender": 2,
          "member_ids": ["123456789012", "123456789011"]
        },
        {
          "membership": "Blue",
          "email": "bob@stamps.co.id",
          "stamps": 15,
          "id": 13,
          "name": "Customer Blue",
          "phone": "+62811231232",
          "address": "Baker Street 221B",
          "gender": 1,
          "member_ids": []
        }
      ]
    }


3. Registration
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/register
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to register your customer through Point of Sales
or other websites. On successful redemption, Stamps will send an email
containing an automatically generated password.

=============== =========== =========================
Parameter       Required    Description
=============== =========== =========================
token           Yes         Authentication string
merchant        Yes         Integer indicating merchant ID
name            Yes         Customer's name
email           Yes         Customer's email
mobile_number   Yes         Customer's mobile number
birthday        Yes         Customer's birthday (with format YYYY-MM-DD)
gender          Yes         Customer's gender ("male" or "female")
store           Yes         Integer representing store ID where customer is registered
member_id       No          Customer's member (card) id
address         No          Customer's address
district        No          Customer's address district ID
postal_code     No          Customer's postal code
is_active       No          Customer's registration status
religion        No          Customer's religion
marital_status  No          Customer's marital status
wedding_date    No          Customer's weidding date
extra_data      No          Extra data related to customer

=============== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/register -i -d '{"token": "secreet", "name": "customer", "email": "customer@stamps.co.id", "mobile_number": "+6281314822365", "birthday": "1991-10-19", "gender": "female", "merchant": 788, "address": "221b Baker Street", "store": 412, "is_active": true}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "id": "123",
        "name": "Customer",
        "gender": "male",
        "address": "Jl MK raya",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "phone": "+62812398712",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1989-10-1",
        "postal_code": "10310",
        "protected_redemption": true,
        "religion": 1,
        "marital_status": 1,
        "wedding_date": null,
        "membership": {
          "tags": [],
          "status": 100,
          "status_text": "Blue",
          "stamps": 401,
          "balance": 150000,
          "is_blocked": false,
          "referral_code": "ABCDE",
          "start_date": "2014-08-08",
          "created": "2014-08-08",
          "extra_data": {}
        },
        "location": {
           "district": {"id": 1, "name": "Kebayoran Baru"},
           "regency": {"id": 1, "name": "Jakarta Selatan"},
           "province": {"id": 1, "name": "DKI Jakarta"}
        }
    }





E. Legacy API
-------------

Legacy endpoint's documentation is available at `Legacy Membership API <http://docs.stamps.co.id/en/latest/legacy_customer_api.html>`_



4. Change Member Info
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/change-profile
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to update your customer's profile through Point of Sales
or other websites.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         Customer's integer primary key or Card number
token         Yes         Authentication string
merchant      Yes         Integer indicating merchant ID
name          Yes         Customer's name
birthday      Yes         Customer's birthday (with format YYYY-MM-DD)
gender        Yes         Customer's gender ("male" or "female")
email         No          Customer's email
mobile number No          Customer's phone number
address       No          Customer's address
district      No          Customer's address district ID
postal_code   No          Customer's postal code
extra_data    No          Extra data related to customer
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/change-profile -i -d '{ "token": "secret", "user": 123, "name": "me", "email": "me@mail.com", "mobile_number": "+62215600010", "birthday": "1991-10-19", "gender": "female", "merchant": 14, "address": "221b Baker Street"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "id": "123",
        "name": "Customer",
        "gender": "male",
        "address": "Jl MK raya",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1989-10-1",
        "phone": "+62812398712",
        "postal_code": "10310",
        "protected_redemption": true,
        "religion": 1,
        "marital_status": 1,
        "wedding_date": null
    }



E. Legacy API
-------------

Legacy endpoint's documentation is available at `Legacy Membership API <http://docs.stamps.co.id/en/latest/legacy_customer_api.html>`_

