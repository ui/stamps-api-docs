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
tags                Tags associated with the customer user
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
            "balance": 0,
            "created": "2022-10-18",
            "is_blocked": false,
            "level": 0,
            "level_text": "Silver",
            "member_status": "Active",
            "referral_code": "7J2K1",
            "stamps": 0,
            "start_date": "2022-10-18",
            "tags": [],
            "primary_card": {
                "id": 7,
                "number": "A5MHG",
                "is_active": true,
                "activated_time": null
            }
        },
        "tags": [],
        "user": {
            "address": "Jalan Anggrek No. 1",
            "birthday": "1993-05-30",
            "email": "johndoe@gmail.com",
            "email_is_verified": false,
            "gender": "female",
            "has_incorrect_email": true,
            "has_pin": false,
            "pin_is_blocked": false,
            "has_password": true,
            "id": "454",
            "id_card_file_name": "",
            "id_number": null,
            "is_active": true,
            "is_anonymized": false,
            "marital_status": 1,
            "member_ids": ["RRROWO5U51"],
            "name": "Wisesi",
            "notes": "",
            "phone": "+628555756061",
            "phone_is_verified": false,
            "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
            "pin_is_blocked": false,
            "postal_code": "10310",
            "protected_redemption": false,
            "referral_code": "SR2KORUB",
            "registration_status": "Partial",
            "religion": 1,
            "wedding_date": null,
            "site_activity": {
                "id": 1,
                "registration_method": "API"
            }
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

============================ =========== =========================
Parameter                    Required    Description
============================ =========== =========================
token                        Yes         Authentication string
merchant                     Yes         Integer indicating merchant ID
name                         Yes         Customer's name
email                        Yes         Customer's email
mobile_number                Yes         Customer's mobile number
birthday                     Yes         Customer's birthday (with format YYYY-MM-DD)
gender                       Yes         Customer's gender ("male" or "female")
store                        Yes         Integer representing store ID where customer is registered
member_id                    No          Customer's member (card) id
address                      No          Customer's address
district                     No          Customer's address district ID
postal_code                  No          Customer's postal code
password                     No          Customer's password used to login
referral_code                No          Referal code used to register customer
is_active                    No          Customer's registration status
religion                     No          Customer's religion
marital_status               No          Customer's marital status
wedding_date                 No          Customer's weidding date
extra_data                   No          Extra data related to customer
registration_method          No          :ref:`Registration method <Registration Method>` code, defaults to `API`
registering_employee_code    No          String indicating employee code, will create a new employee if not exists
generate_default_password    No          Boolean, whether to generate a random, default password for the member, defaults to `true`
============================ =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/register -i -d '{"token": "secreet", "name": "customer", "email": "customer@stamps.co.id", "mobile_number": "+6281314822365", "birthday": "1991-10-19", "gender": "female", "merchant": 788, "address": "221b Baker Street", "store": 412, "is_active": true, "registering_employee_code": "EMP001"}'


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
        "id": "620",
        "name": "John Doe",
        "gender": "male",
        "address": "Jalan Anggrek No. 1",
        "is_active": true,
        "email": "johndoe@example.com",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1993-05-30",
        "phone": "+6285567146065",
        "postal_code": "10310",
        "protected_redemption": false,
        "has_incorrect_email": true,
        "marital_status": 1,
        "religion": 1,
        "wedding_date": null,
        "id_number": null,
        "id_card_file_name": "",
        "phone_is_verified": false,
        "email_is_verified": false,
        "is_anonymized": false,
        "has_pin": false,
        "pin_is_blocked": false,
        "has_password": true,
        "notes": "",
        "referral_code": "GYHTLIY9",
        "registration_status": "Full",
        "location": {
            "district": {
                "id": 1,
                "name": "Kebayoran Baru"
            },
            "regency": {
                "id": 1,
                "name": "Jakarta Selatan"
            },
            "province": {
                "id": 1,
                "name": "DKI Jakarta"
            }
        },
        "membership": {
            "tags": [],
            "status": 0,
            "status_text": "Silver",
            "level": 0,
            "level_text": "Silver",
            "member_status": "Active",
            "stamps": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "7J133",
            "start_date": "2022-11-24",
            "created": "2022-11-24",
            "primary_card": {
                "id": 231,
                "number": "RRRB1AKUT0",
                "is_active": true,
                "activated_time": "2022-01-20 10:00:00"
            }
        },
        "registering_employee_code": "EMP001"
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

============================ =========== =========================
Parameter                    Required    Description
============================ =========== =========================
user                         Yes         Customer's integer primary key or Card number
token                        Yes         Authentication string
merchant                     Yes         Integer indicating merchant ID
name                         Yes         Customer's name
birthday                     No          Customer's birthday (with format YYYY-MM-DD)
gender                       No          Customer's gender ("male" or "female")
address                      No          Customer's address
district                     No          Customer's address district ID
postal_code                  No          Customer's postal code
extra_data                   No          Extra data related to customer
has_downloaded_app           No          Boolean indicating user has downloaded an app
phone_is_verified            No          Boolean indicating user's phone is verified
email_is_verified            No          Boolean indicating user's email is verified
notes                        No          String to give custom notes to this user
marital_status               No          Customer's marital status
wedding_date                 No          Customer's weidding date
generate_default_password    No          Boolean, whether to generate a random, default password for the member, defaults to `true`
============================ =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/change-profile -i -d '{ "token": "secret", "user": 123, "name": "me", "email": "me@mail.com", "mobile_number": "+62215600010", "birthday": "1991-10-19", "gender": "female", "merchant": 14, "address": "221b Baker Street" "phone_is_verified": true, "notes": "A note"}'


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
        "wedding_date": null,
        "is_anonymized": true,
    }



E. Legacy API
-------------

Legacy endpoint's documentation is available at `Legacy Membership API <http://docs.stamps.co.id/en/latest/legacy_customer_api.html>`_



5. Get Full Profile
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/full-profile
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to get your full customer's profile.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
token         Yes         Authentication string
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X GET -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/full-profile -i -d '{ "token": "secret", "user": 123}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                Customer profile data
tags                Tags associated with customer's membership
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
        "user": {
            "id": 319,
            "name": "John Doe",
            "is_active": true,
            "phone": "+6281343283754",
            "email": "johndoe@example.com",
            "has_incorrect_email": false,
            "has_incorrect_phone": false,
            "has_incorrect_wa_number": false,
            "phone_is_verified": false,
            "email_is_verified": false,
            "is_anonymized": false,
            "has_pin": false,
            "pin_is_blocked": false,
            "has_password": true,
            "address": "Anggrek 5 No. 1",
            "birthday": "1999-07-15",
            "gender": "f",
            "location": {
                "district": {
                    "id": 1,
                    "name": "Kembangan"
                },
                "regency": {
                    "id": 2,
                    "name": "Jakarta Barat"
                },
                "province": {
                    "id": 3,
                    "name": "Jakarta"
                }
            },
            "religion": "Budha",
            "marital_status": "Married",
            "wedding_date": "2023-03-03",
            "nationality": "Indonesian",
            "postal_code": "11610",
            "hobbies": [
                {
                    "id": 30,
                    "code": "28",
                    "name": "Film"
                },
                {
                    "id": 32,
                    "code": "30",
                    "name": "Games"
                }
            ],
            "children": [
                {
                    "id": 14,
                    "name": "Iris",
                    "gender": "f",
                    "birthday": "2000-10-19"
                }
            ],
            "pets": [
                {
                    "id": 5,
                    "name": "Rocky",
                    "birthday": "1991-10-19",
                    "gender": "f",
                    "type": {
                        "code": "79",
                        "name": "Dog"
                    },
                    "breed": {
                        "code": "1",
                        "name": "Affenpinscher"
                    }
                },
                {
                    "id": 3,
                    "name": "Sandy",
                    "birthday": "1991-10-19",
                    "gender": "f",
                    "type": {
                        "code": "78",
                        "name": "Cat"
                    },
                    "breed": {
                        "code": "18",
                        "name": "Fox Terrier"
                    }
                }
            ],
            "social_media_profile": {
                "twitter": "@twitter",
                "instagram": "@instagram",
                "facebook": ""
            },
            "notes": "Update HP Required",
            "has_downloaded_app": false
        },
        "tags": [
            {
                "group": "category",
                "tag": "vvip"
            }
        ]
    }


6. Level Upgrade Requirement
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
upgrade_requirement   Customer's upgrade requirement
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
      "upgrade_requirement": {
          "spending_requirement": 590000,
          "deadline": "2022-12-31",
          "next_level": "Silver"
      }
    }


7. Add Membership Tag
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/add-key-value-tag
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to add a tag to your customer's membership.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         Customer's integer primary key or Card number
token         Yes         Authentication string
merchant      Yes         Integer indicating merchant ID
key           Yes         Tag key name
value         Yes         Tag value name
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/add-key-value-tag -i -d '{ "token": "secret", "user": 123, "merchant": 14, "key": "category", "value": "vvip"}'


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
        "tags": ["vvip"],
        "status": 1,
        "status_text": "Blue",
        "stamps": 100,
        "balance": 100,
        "is_blocked": false,
        "referral_code": "ABCDEF",
        "start_date": "2016-02-31",
        "created": "2016-02-14",
        "extra_data": {},
    }


8. Remove Membership Tag
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/remove-tag
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to add a tag to your customer's membership.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         Customer's integer primary key or Card number
token         Yes         Authentication string
merchant      Yes         Integer indicating merchant ID
key           Yes         Tag key name
value         Yes         Tag value name
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/remove-tag -i -d '{ "token": "secret", "user": 123, "merchant": 14, "key": "category", "value": "vvip"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              status
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
        "status": "ok"
    }


9. Set social media profile
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/set-social-media-profile
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to set customer's social media profile.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         Customer's integer primary key or Card number
token         Yes         Authentication string
facebook      No          String, field will be unchanged if not supplied
twitter       No          String, field will be unchanged if not supplied
instagram     No          String, field will be unchanged if not supplied
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/set-social-media-profile -i -d '{ "token": "secret", "user": 123, "instagram": "", "twitter": "@test"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              status
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
        "facebook": "Test",
        "instagram": "",
        "twitter": "@test"
    }


10. Anonymize Customer
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/anonymize
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to anonymize customer.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
identifier    Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/anonymize -i -d '{ "token": "secret", "identifier": 123}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              status
=================== ==============================


C. Response Codes
-----------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized - Often missing or
                    wrong authentication token
403                 Forbidden - You do not have
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
        "status": "ok"
    }


11. Set Level
===============
| URL endpoint: https://stamps.co.id/api/v2/memberships/set-level
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to override customer's level.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
token         Yes         Authentication string
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
level         Yes         A level numerical value
upgrade_only  No          A boolean value, default to false. If set to true, ``level`` must be higher than user's current level
end_date      No          If set, ``level`` will only be set until the specified date. Must not be earlier than today.
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/memberships/set-level -i -d '{ "token": "secret", "user": 123, "level": 200, "end_date": "2023-12-31"}'


B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
=================== ==============================


C.  Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST
    [Redacted Header]
    {
        "status": "ok"
    }

The customer does not have membership:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "user: User does not have membership in Your Merchant",
        "errors": {
            "user": "User does not have membership in Your Merchant"
        },
        "error_code": "user_has_no_membership",
        "error_message": "User does not have membership in Your Merchant"
    }

``upgrade_only`` is set to true, but ``level`` is lower than user's current level:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "__all__: Level is lower than user's current level and downgrade is not allowed",
        "error_message": "Level is lower than user's current level and downgrade is not allowed",
        "error_code": "downgrade_not_allowed",
        "errors": {
            "__all__": "Level is lower than user's current level and downgrade is not allowed"
        }
    }


12. Request OTP to Modify Mobile Number
===============
| URL endpoint: https://stamps.co.id/api/v2/accounts/request-change-mobile-number-otp
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to request authentication code for change mobile number.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
mobile_number Yes         A string indicating customer new mobile number
type          Yes         A choices for delivery channel for otp ( sms, whatsapp )
template_code Yes         A template code for otp messages template, can be setup in merchant interfaces
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/accounts/request-change-mobile-number-otp -i -d '{ "token": "secret", "identifier": 123, "type": "sms", "template_code": "OTP_1"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
otp                 otp number for authentication
=================== ==============================


C. Response Codes
-----------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized - Often missing or
                    wrong authentication token
403                 Forbidden - You do not have
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
        "otp": "1234"
    }

Invalid Template Code:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "template_code: Messages template not found",
        "errors": {
            "template_code": "Messages template not found"
        },
        "error_code": "invalid_template_code",
        "error_message": "Messages template not found"
    }

Invalid Whatsapp number if delivery channel is whatsapp

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "type: User does not have mobile number or invalid whatsapp number",
        "errors": {
            "type": "User does not have mobile number or invalid whatsapp number"
        },
        "error_code": "invalid_user_whatsapp_number",
        "error_message": "User does not have mobile number or invalid whatsapp number"
    }


13. Modify Mobile Number
===============
| URL endpoint: https://stamps.co.id/api/v2/accounts/change-mobile-number
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to modify mobile number.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
identifier    Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
new_number    Yes         A new mobile number
otp           Yes         A string for authentication
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/accounts/change-mobile-number -i -d '{ "token": "secret", "identifier": 123, "new_number": "+628123454321", "otp": "1234"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              status
=================== ==============================


C. Response Codes
-----------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized - Often missing or
                    wrong authentication token
403                 Forbidden - You do not have
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
        "status": "ok"
    }


14. Request OTP to Modify Email
===============================
| URL endpoint: https://stamps.co.id/api/v2/accounts/request-otp-for-email
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to request authentication code to change email. The authentication code will be sent to the new email address provided.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
new_email     Yes         A string indicating customer's new email address
template_code No          A template code for otp messages template, can be setup in merchant interfaces
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/accounts/request-otp-for-email -i -d '{ "token": "secret", "new_email": "alice@stamps.co.id", "template_code": "OTP_1"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
otp                 6 digit string OTP number for authentication
=================== ==============================


C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    {
        "otp": "123456"
    }

Invalid Template Code:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "template_code: Email template not found",
        "errors": {
            "template_code": "Email template not found"
        },
        "error_code": "invalid_template_code",
        "error_message": "Email template not found"
    }


15. Modify Email
================
| URL endpoint: https://stamps.co.id/api/v2/accounts/change-email
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to modify email.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
otp           Yes         6 digit string OTP received from ``Request OTP to Modify Email`` API
new_email     Yes         A new email
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/accounts/change-email -i -d '{ "token": "secret", "user": 123, "new_email": "alice@stamps.co.id", "otp": "123456"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
=================== ==============================


C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    {
        "status": "ok"
    }

Email is already used by other user

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "new_email: alice@stamps.co.id is already used",
        "errors": {
            "new_email": "alice@stamps.co.id is already used",
        },
        "error_code": "email_already_used",
        "error_message": "alice@stamps.co.id is already used"
    }


16. Partial Registration
===============
| URL endpoint: https://stamps.co.id/api/v3/memberships/partial-registration
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to register your customer

============================ =========== =========================
Parameter                    Required    Description
============================ =========== =========================
token                        Yes         Authentication string
user                         Yes         Member's email or mobile number
send_registration_message    No          Default True
registering_employee_code    No          A string indicating
                                         employee code
============================ =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v3/memberships/partial-registration -i -d '{"token": "secreet", "user": "partial1@mail.com"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                Various user data
membership          Various membership data
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
        "membership": {
            "level": 100,
            "level_text": "Blue",
            "status": "Active",
            "stamps": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "7LXJ7",
            "start_date": "2022-09-16",
            "created": "2022-09-16",
            "primary_card": {
                "id": 7,
                "number": "A5MHG",
                "is_active": true,
                "activated_time": null
            }
        },
        "user": {
            "id": 140,
            "name": "",
            "gender": null,
            "is_active": true,
            "email": "partial1@mail.com",
            "birthday": null,
            "picture_url": null,
            "phone": null,
            "has_incorrect_email": false,
            "has_incorrect_phone": false,
            "has_incorrect_wa_number": false,
            "phone_is_verified": false,
            "email_is_verified": false,
            "registering_employee_code": "ABC123",
            "member_ids": [
                "RRRAWWROWO",
                "RRRAWRRUWU"
            ]
            "primary_membership_id": "RRRAWWROWO"
        }
    }


Miscellaneous
------------------------------

Registration Method
^^^^^^^^^^^^^^^^^^^^^
=================== ===========
Code                Description
=================== ===========
0                   Mobile Web
1                   Web
2                   Store
3                   API
4                   Merchant
5                   Mobile API
6                   SMS
9                   Android
10                  iOS
12                  Imported
=================== ===========
