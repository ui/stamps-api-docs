************************************
Legacy Membership API
************************************

1. Validate Legacy Membership
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/validate
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can validate whether a legacy membership exists and check its status by calling this API.

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
merchant_id      Yes         Merchant ID the legacy member is associated with
member_id        No          String indicating membership's ID. Required when mobile number and email are not provided
mobile_number    No          Required together with email
email            No          Required together with mobile number
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -H 'Authorization: <token_type> <token>' 'https://stamps.co.id/api/legacy/members/validate?merchant_id=1&member_id=322145'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

====================== ==============================
Variable               Description
====================== ==============================
legacy_member          Various information about legacy member.
                       Refer :ref:`here <Legacy Member Status>` for member status
errors                 Errors encountered when processing request (if any)
====================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "legacy_member": {
        "member_id": "322145",
        "email": "legacy_member@stamps.com",
        "mobile_number": "+62851111222333",
        "merchant": 1,
        "status": 1,
        "duplicate_id": "1234"
      }
    }


2. Validate Legacy Membership Pin
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/validate-pin
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can validate whether a legacy membership exists and check its status by calling this API.

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
user             Yes         A string indicating legacy member's ID, mobile number or email
pin              Yes         User's pin
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/legacy/members/validate-pin -i -d '{ "user": "legacy_member@stamps.com", "pin": "123456"}'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

====================== ==============================
Variable               Description
====================== ==============================
status                 Status of the request
====================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "status": "ok"
    }


3. Get Duplicate Legacy Members
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/check-duplicates
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can get unmerged legacy memberships that are potentially duplicate to a legacy member with this API.

============     =========== =========================
Parameter        Required    Description
============     =========== =========================
member_id        Yes         Legacy membership's ID
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -H 'Authorization: <token_type> <token>' 'https://stamps.co.id/api/legacy/check-duplicates?member_id=612345'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
legacy_members      An array of legacy member objects
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

    {
      "legacy_members": [
        {
          "member_id": "322145",
          "email": "legacy_member@stamps.com",
          "mobile_number": "+62851111222333",
          "merchant": 1,
          "status": 2,
          "duplicate_id": "1234"
        },
        {
          "member_id": "63414",
          "email": "duplicate_member2@stamps.com",
          "mobile_number": "+62851111222444",
          "merchant": 2,
          "status": 2,
          "duplicate_id": "1234"
        },
      ],
      "merged_legacy_members": [
        {
          "member_id": "377142",
          "email": "legacy_member3@stamps.com",
          "mobile_number": "+62851111222555",
          "merchant": 1,
          "status": 1,
          "duplicate_id": "1234"
        }
      ]
    }



4. Search Legacy Membership
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/search
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can search for legacy members using one or a combination of these identifiers: mobile number, email, or member ID.

======================= =========== =========================
Parameter               Required    Description
======================= =========== =========================
email                   No          Membership email. Required if other identifiers are empty.
mobile_number           No          Membership mobile number. Required if other identifiers are empty.
member_id               No          Membership member ID. Required if other identifiers are empty.
check_duplicate_id      No          Boolean indicating whether to check members' duplicate ID or not.
======================= =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -H 'Authorization: <token_type> <token>' 'https://stamps.co.id/api/legacy/members/search?email=test@stamps.co.id&mobile_number=+6285123123123'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
legacy_members      An array of legacy member objects
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

    {
      "legacy_members": [
        {
          "member_id": "322145",
          "email": "legacy_member@stamps.com",
          "mobile_number": "+62851111222333",
          "merchant": 1,
          "status": 1,
          "duplicate_id": "1234"
        },
        {
          "member_id": "63414",
          "email": "duplicate_member2@stamps.com",
          "mobile_number": "+62851111222444",
          "merchant": 2,
          "status": 1,
          "duplicate_id": "1234"
        },
      ],
      "merged_legacy_members": []
    }



5. Merge Legacy Membership
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/merge
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can merge a legacy membership to a stamps membership with this API

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
target_user      Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
legacy_member    Yes         A string indicating legacy member's ID, mobile number or email
pin              Yes         Legacy member's pin
merchant_id      Yes         Merchant ID the legacy member is associated with
bonus_stamps     No          Integer, bonus points given to target user's membership
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/legacy/members/merge -i -d '{ "target_user": 1, "legacy_member": 31245, "pin": "123456", "merchant_id": 1, "bonus_stamps": 10 }'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
membership          Various information about target user's membership
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

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
        "stamps": 410,
        "balance": 150000,
        "is_blocked": false,
        "referral_code": "ABCDE",
        "start_date": "2014-08-08",
        "created": "2014-08-08"
      }
    }


6. Request Pin
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/request-pin
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
Send legacy member's pin by email or SMS

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
type             Yes         Type of pin request. Can be `email`, `whatsapp`` or `sms`
user             Yes         A string indicating legacy member's ID, mobile number or email
merchant_id      Yes         Merchant ID the legacy member is associated with
template_code    No          Template code for the pin request
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/legacy/members/request-pin -i -d '{ "type": "email", "user": "test@example.com", "merchant_id": 1, "template_code": "1234" }'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              Status of the request
=================== ==============================


C. Example Response
-------------------

On successful request pin:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "status": "ok"
    }


7. Activate Legacy Membership
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/activate
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
This API turns a legacy member data into to an active membership.

============================ =========== =========================
Parameter                    Required    Description
============================ =========== =========================
user                         Yes         A string indicating legacy member's ID, mobile number or email
merchant_id                  Yes         Merchant ID the legacy member is associated with
pin                          Yes         Legacy member's pin
store                        No          Store ID or name the legacy member is being activated from
bonus_stamps                 No          Integer, bonus points given to target user's membership
employee_code                No          Employee code
generate_default_password    No          Boolean, whether to generate a random, default password for the member, defaults to `true`
============================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/legacy/members/activate -i -d '{ "user": 12, "pin": "123456", "merchant_id": 1, "bonus_stamps": 10 }'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                Customer profile data
membership          Various information about active membership
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "user": {
        "id": "123",
        "name": "Customer",
        "gender": "m",
        "address": "Jl MK raya",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "phone": "+62812398712",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1989-10-1"
      },
      "membership": {
        "level": 1,
        "level_text": "Blue",
        "stamps": 100,
        "balance": 0,
        "is_blocked": false,
        "referral_code": "abc123",
        "start_date": "2022-01-01",
        "created": "2022-01-01",
        "primary_card": {
          "id": 1,
          "number": "RRR123456",
          "is_active": true,
          "activated_time": "2022-01-20 10:00:00"
        }
      }
    }


8. Get Merged Legacy Members
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/get-merged-members
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
Allows you to query for legacy members that have been merged into a user's membership account.

============     =========== =========================
Parameter        Required    Description
============     =========== =========================
user             Yes         A string indicating customer's email, Member ID, mobile number or primary key ID.
                             This should be an active membership account.
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -H 'Authorization: <token_type> <token>' 'https://stamps.co.id/api/legacy/get-merged-members?user=2'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
merged_members      An array of legacy member objects
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
      "merged_members": [
        {
          "member_id": "322177",
          "email": "merged_legacy_member@stamps.com",
          "mobile_number": "+62851111222444",
          "merchant": 1,
          "status": 1,
          "duplicate_id": "1234"
        }
      ]
    }


Miscellaneous
------------------------------

Legacy Member Status
^^^^^^^^^^^^^^^^^^^^
=================== ===========
Code                Description
=================== ===========
1                   Merged
2                   Unmerged
=================== ===========
