************************************
Custom KLG API
************************************


1. Get Duplicate Legacy Members
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/check-duplicates-with-pin
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can get unmerged legacy memberships that are potentially duplicate to a legacy member with this API.

============     =========== =========================
Parameter        Required    Description
============     =========== =========================
token            Yes         Authentication token in string
user             Yes         Legacy membership's email or mobile number
pin              Yes         Legacy mebership's PIN
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/legacy/check-duplicates-with-pin?token=123&user=example@stamps.com&pin=123456'


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
