************************************
Membership API
************************************

1. Getting Member Data
=======================================
| URL endpoint: https://stamps.co.id/api/memberships/status
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
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    # Please note that for cURL command you need to escape special characters
    $ curl 'https://stamps.co.id/api/memberships/status?token=abc&user=customer@stamps.co.id&merchant=14'


B. Response Data
----------------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
name                Member's name
birthday            Member's birthday (if known)
stamps              Total stamps this user has
email               Member's email
identifier          Member's identifier (can be email or phone)
membership_status   Membership status of the user
is_active           Whether user is registered on Stamps
member_ids          List of card numbers associated with member
gender              "1" means male, "2" means female
phone               Member's phone number (if any)
address             Member's address (if any)
balance             Amount of member's Stamps balance
detail              Description of error (if any)
validation_errors   Errors encountered when parsing
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
      "id": 2456,
      "stamps": 10,
      "membership_status": "Gold",
      "phone": "+6281111111",
      "birthday": "2000-09-15",
      "address": "Baker Street 221B",
      "name": "Alice",
      "gender": 2,
      "member_ids": ["123456789012", "123456789011"],
      "balance": 2000
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
| URL endpoint: https://stamps.co.id/api/memberships/register
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to register your customer through Point of Sales
or other websites. On successful redemption, Stamps will send an email
containing an automatically generated password.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
merchant    Yes         Integer indicating merchant ID
name        Yes         Customer's name
email       Yes         Customer's email
birthday    Yes         Customer's birthday (with format YYYY-MM-DD)
gender      Yes         Customer's gender ("male" or "female")
store       Yes         Integer representing store ID where customer is registered
member_id   No          Customer's member (card) id
phone       No          Customer's phone number
address     No          Customer's address
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/memberships/register -i -d '{ "token": "secret", "name": "me", "email": "me@mail.com", "member_id": "123412341234", "phone": "+62215600010", "birthday": "1991-10-19", "gender": "Female", "merchant": 14, "address": "221b Baker Street", "store": 2}'


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
      "customer": {
        "id": 3,
        "name": "me",
        "email": "me@mail.com",
        "identifier": "me@mail.com"
        "member_id": "123412341234",
        "phone": "0215600010",
        "birthday": "1991-10-19",
        "gender": "Female",
      }
    }



4. Change Member Info
===============
| URL endpoint: https://stamps.co.id/api/memberships/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to update your customer's profile through Point of Sales
or other websites.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
id          Yes         Customer's integer primary key ID
token       Yes         Authentication string
merchant    Yes         Integer indicating merchant ID
name        Yes         Customer's name
birthday    Yes         Customer's birthday (with format YYYY-MM-DD)
gender      Yes         Customer's gender ("male" or "female")
email       No          Customer's email
member_id   No          Customer's member (card) id
phone       No          Customer's phone number
address     No          Customer's address
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/memberships/change -i -d '{ "token": "secret", "id": 123, "name": "me", "email": "me@mail.com", "member_id": "123412341234", "phone": "+62215600010", "birthday": "1991-10-19", "gender": "Female", "merchant": 14, "address": "221b Baker Street"}'


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
      "customer": {
        "id": 3,
        "name": "me",
        "email": "me@mail.com",
        "member_id": "123412341234",
        "phone": "0215600010",
        "birthday": "1991-10-19",
        "gender": "Female",
      }
    }



4. Upgrade Requirement
===============
| URL endpoint: https://stamps.co.id/api/memberships/upgrade-requirement
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to get your customer's upgrade requirement.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
user        Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
token       Yes         Authentication string
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/memberships/upgrade-requirement?token=secret&user=me@mail.com'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

===================== ==============================
Variable              Description
===================== ==============================
membership            Various customer data
upgrade_requirement   Customer's upgrade requirement
benefit_rewards       Customer's current level benefits
===================== ==============================


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
      },
      "upgrade_requirement": {
          "spending_requirement": 590000,
          "deadline": "2022-12-31",
          "next_level": "Silver"
      },
      "benefit_rewards": [
          {
            "stamps_to_redeem": 20,
            "image_url": "http://foo.com",
            "landscape_url": null,
            "id": 6,
            "membership": "Blue",
            "name": "Mee Goreng",
            "description": "reward description",
            "price": null,
            "extra_data": {
              "SKU": "A001SKU"
            },
            "merchant_code": null,
            "redeemable": true,
            "is_visible": true,
            "terms": "",
            "code": "A001",
            "type": 3
          },
      ]
    }
