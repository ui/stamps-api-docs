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

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/set -i -d '{ "token": "secret", "user": 123, "pin": "123456", "confirm_pin": "123456" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Mismatch ``pin`` and ``confirm_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "confirm_pin: Confirmation PIN does not match",
        "errors": {
            "confirm_pin": "Confirmation PIN does not match"
        },
        "error_code": "mismatch_pin",
        "error_message": "confirm_pin: Confirmation PIN does not match"
    }


2. Change PIN
=============
| URL endpoint: https://stamps.co.id/api/pin/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=============== ======== ===========
Parameter       Required Description
=============== ======== ===========
token           Yes      Authentication string
user            Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
current_pin     Yes      Customer's previously set 6 digit string PIN
new_pin         Yes      6 digit string
confirm_new_pin Yes      6 digit string that needs to be the same as ``new_pin`` parameter
=============== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/change -i -d '{ "token": "secret", "user": 123, "current_pin": "123456", "new_pin": "654321", "confirm_new_pin", "654321" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Invalid PIN:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "current_pin: Invalid PIN",
        "errors": {
            "current_pin": "Invalid PIN"
        },
        "error_code": "invalid_pin",
        "error_message": "current_pin: Invalid PIN"
    }

Mismatch ``new_pin`` and ``confirm_new_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "confirm_new_pin: Confirmation PIN does not match",
        "errors": {
            "confirm_new_pin":"Confirmation PIN does not match"
        },
        "error_code": "mismatch_pin",
        "error_message":"confirm_new_pin: Confirmation PIN does not match"
    }


3. Validate PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/validate
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Customer's PIN will be blocked in case of repeated failed validation.

========= ======== ===========
Parameter Required Description
========= ======== ===========
token     Yes      Authentication string
user      Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
pin       Yes      6 digit string
========= ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/validate -i -d '{ "token": "secret", "user": 123, "pin": "123456" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Invalid PIN:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "pin: Invalid PIN",
        "errors": {
            "pin": "Invalid PIN"
        },
        "error_code": "invalid_pin",
        "error_message": "pin: Invalid PIN"
    }


4. Unblock PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/unblock
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Unblock customer's PIN blocked by repeated failed validation

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
token       Yes      Authentication string
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/unblock -i -d '{ "token": "secret", "user": 123 }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }


5. Requesting an OTP to Reset PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/request-otp-for-reset
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

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/request-otp-for-reset -i -d '{ "token": "secret", "user": 123 }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }


6. Reset PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/reset
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

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/pin/reset -i -d '{ "token": "secret", "user": 123, "otp": "123123", "pin": "654321", "confirm_pin", "654321" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Mismatch ``pin`` and ``confirm_pin`` parameter:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "confirm_pin: Confirmation PIN does not match",
        "errors": {
            "confirm_pin": "Confirmation PIN does not match"
        },
        "error_code": "mismatch_pin",
        "error_message": "confirm_pin: Confirmation PIN does not match"
    }

Invalid OTP:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "otp: Invalid OTP",
        "errors": {
            "otp": "Invalid OTP"
        },
        "error_code": "invalid_otp",
        "error_message": "otp: Invalid OTP"
    }
