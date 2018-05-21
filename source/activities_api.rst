************************************
Activities member API
************************************

1. Querying for activities member
=======================================
| URL endpoint: https://stamps.co.id/api/v2/activities
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for 50 activities member

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating customer's email or Member ID
older_than  No         Integer indicating how much activities that will show
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/v2/activities?token=abc&user=customer@stamps.co.id'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ============================== 
Variable            Description
=================== ==============================
activities          List of activities member
=================== ==============================

=================== ============================== =========================
Type                Activities                     Description
=================== ============================== =========================
0                   Transaction                    Earned stamps
1                   Redemption                     Redeemed something
2                   Award                          Awarded Stamps
3                   Twitter Award                  Twitter awarded stamps
4                   Facebook Award                 Facebook awarded stamps
5                   Membership Upgrade             Membership upgrade
6                   Membership Downgrade           Membership downgrade
7                   Change Balance                 Change balance
8                   Survey Submission              Survey submission
=================== ============================== =========================

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
      "activities": [
        {
            "id": 12345,
            "type": 2,
            "created": "2018-05-16T05:50:54+00:00",
            "merchantName": "Merchant",
            "merchantID": 1,
            "stamps": 1,
            "status": 1,
            "name": "test notes"
        },
        {
            "id": 1714718,
            "type": 2,
            "created": "2018-05-11T02:35:57+00:00",
            "merchantName": "Sandbox",
            "merchantID": 57,
            "stamps": 1,
            "status": 1,
            "name": ""
        }
      ]
    }


If missing or wrong authentication token:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Authentication credentials were not provided."}

