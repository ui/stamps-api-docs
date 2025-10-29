************************************
Children API
************************************

1. Add Child
===============
| URL endpoint: https://stamps.co.id/api/children/add
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to add child data to customer.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
user          Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
name          Yes         string
birthday      Yes         YYYY-MM-DD
gender        Yes         "m" or "f"
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/children/add -i -d '{ "user": 123, "name": "child", "birthday": "1991-10-19", "gender": "f"}'


B. Response Data
----------------
Stamps responds to this API call with the following data:

=================== ==============================
Variable            Description
=================== ==============================
child               Created child data
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
        "child": {
            "birthday": "2099-09-09",
            "gender": "f",
            "name": "Child 1",
            "id": 1
        }
    }


2. Delete Child
===============
| URL endpoint: https://stamps.co.id/api/children/delete
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to delete customer's child data.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
child         Yes         To be deleted child ID
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/children/delete -i -d '{ "child": 123 }'


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
