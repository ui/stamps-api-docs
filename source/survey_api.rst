************************************
Survey Api
************************************

1. List Survey
===============
| URL endpoint: https://stamps.co.id/api/surveys/
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for all available survey on customer on stamps

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
token         Yes         Authentication string
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X GET -H "Content-Type: application/json" https://stamps.co.id/api/surveys/ -i -d '{ "token": "secret", "user": 123}'


B. Response Data
----------------
Stamps responds to this API call with the following data:

=================== ==============================
Variable            Description
=================== ==============================
surveys             list of available surveys
                    contains id, name, merchant_id,
                    description, and personal_link
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
        "surveys": [
            {
                "id": 28,
                "name": "Vaughan Sosa",
                "merchant_id": 1,
                "personal_url": "http://stamps.co.id/proxy/add-survey/28/VS8i6JnqR5at3ZipgPLSC4",
                "description": "Dolor quia soluta en"
            },
            {
                "id": 20,
                "name": "Julie Terrell",
                "merchant_id": 1,
                "personal_url": "http://stamps.co.id/proxy/add-survey/20/VS8i6JnqR5at3ZipgPLSC4",
                "description": "Quia aut maiores nat"
            }
        ]
    }


