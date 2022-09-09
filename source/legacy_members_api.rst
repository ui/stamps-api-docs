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
token            Yes         Authentication string
merchant_id      Yes         Merchant ID the legacy member is associated with
member_id        No          String indicating membership's ID. Required when mobile number and email are not provided
mobile_number    No          Required together with email
email            No          Required together with mobile number
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/legacy/members/validate?token=123&merchant_id=1&member_id=322145'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
member_id           Legacy membership's ID
merchant            Merchant ID which the member is associated to
status              `1` means legacy member has been merged, while
                    `2` means legacy member is unmerged
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
      "member_id": "322145",
      "email": "legacy_member@stamps.com",
      "mobile_number": "+62851111222333",
      "merchant": 1,
      "status": 1
    }


2. Get Duplicate Legacy Members
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
token            Yes         Authentication token in string
member_id        Yes         Legacy membership's ID
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/legacy/check-duplicates?token=123&member_id=612345'


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

On successful balance update:

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
          "status": 1
        },
        {
          "member_id": "63414",
          "email": "duplicate_member2@stamps.com",
          "mobile_number": "+62851111222444",
          "merchant": 2,
          "status": 1
        },
      ]
    }



3. Merge Legacy Membership
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
token            Yes         Authentication string
target_user      Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
legacy_member    Yes         A string indicating legacy member's ID, mobile number or email
pin              Yes         Legacy member's pin
merchant_id      Yes         Merchant ID the legacy member is associated with
bonus_stamps     No          Integer, bonus points given to target user's membership
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/legacy/members/merge -i -d '{ "token": "secret", "target_user": 1, "legacy_member": 31245, "pin": "123456", "merchant_id": 1, "bonus_stamps": 10 }'



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

On successful balance update:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "status": 100,
      "status_text": "Blue",
      "stamps": 410,
      "balance": 150000,
      "is_blocked": false,
      "referral_code": "ABCDE",
      "start_date": "2014-08-08",
      "created": "2014-08-08",
      "extra_data": {},
    }


4. Request Pin
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
token            Yes         Authentication string
type             Yes         Type of pin request. Can be `email` or `sms`
merchant_id      Yes         Merchant ID the legacy member is associated with
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/legacy/members/request-pin -i -d '{ "token": "secret", "type": "email", "merchant_id": 1 }'



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
