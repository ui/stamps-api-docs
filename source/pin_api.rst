*******
PIN API
*******

1. Set PIN
==========
| URL endpoint: https://stamps.co.id/api/pin/set
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Set customer's PIN for the first time.

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
token       Yes      Authentication string
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/set -i -d '{ "token": "secret", "user": 123, "pin": "123456"}'

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              status (``ok``, ``mismatch``)
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

A successful API call with mismatch ``pin`` and ``confirm_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "mismatch"
    }



2. Change PIN
=============
| URL endpoint: https://stamps.co.id/api/pin/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
token       Yes      Authentication string
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
old_pin     Yes      Customer's previously set 6 digit string PIN
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/change -i -d '{ "token": "secret", "user": 123, "old_pin": "123456", "pin": "654321"}'

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              status (``ok``, ``mismatch``)
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

A successful API call with mismatch ``pin`` and ``confirm_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "mismatch"
    }


3. Validate PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/validate
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

========= ======== ===========
Parameter Required Description
========= ======== ===========
token     Yes      Authentication string
user      Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
pin       Yes      6 digit string
========= ======== ===========

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              status (``ok``, ``invalid``)
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

A successful API call with invalid PIN:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "invalid"
    }


4. Forgot PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/forgot
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Request an OTP to unblock customer's PIN

========= ======== ===========
Parameter Required Description
========= ======== ===========
token     Yes      Authentication string
user      Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
========= ======== ===========

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              status (``ok``)
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


5. Unblock PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/unblock
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
token       Yes      Authentication string
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
otp         Yes      6 digit string OTP received from ``Forgot PIN`` API
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              status (``ok``, ``mismatch``, ``invalid``)
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

A successful API call with mismatch ``pin`` and ``confirm_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "mismatch"
    }

A successful API call with invalid OTP:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "status": "invalid"
    }
