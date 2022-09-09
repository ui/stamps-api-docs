************************************
Pet API
************************************

1. Add Pet
===============
| URL endpoint: https://stamps.co.id/api/pets/add
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to add pet data to customer.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
token         Yes         Authentication string
name          Yes         string
type          Yes         "cat" or "dog" or etc
birthday      No          YYYY-MM-DD
breed         No          A string indicating pet's breed code, ex: "persia", "shiba", etc
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pets/add -i -d '{ "token": "secret", "user": 123, "name": "Kat", "birthday": "1991-10-19", "type": "cat"}'


B. Response Data
----------------
Stamps responds to this API call with the following data:

=================== ==============================
Variable            Description
=================== ==============================
pet                 Created pet data
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
        "pet": {
            "id": 1,
            "name": "Kat",
            "birthday": "1989-04-15",
            "type": {
                "code": "cat",
                "name": "Felines"
            },
            "breed": {
                "code": "persia",
                "name": "Persian"
            },
        }
    }


2. Delete Pet
===============
| URL endpoint: https://stamps.co.id/api/pets/delete
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to delete customer's pet data.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
token         Yes         Authentication string
pet           Yes         To be deleted pet ID
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pets/delete -i -d '{ "token": "secret", "pet": 123 }'


B. Response Data
----------------
Stamps responds to this API call with the following data:

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
