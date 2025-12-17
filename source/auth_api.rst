*******
Auth API
*******

1. Validate Password
====================================
| URL endpoint: https://stamps.co.id/api/auth/validate-password
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
user        Yes         A string indicating user's email address or member ID
password    Yes         User's password
=========== =========== =========================

Here's an example of how the API call might look like in JSON format

.. code-block :: bash

    {
        "user": "customer@stamps.co.id",
        "password": "secret123"
    }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" -d '{"user": "customer@stamps.co.id", "password": "secret123"}' https://stamps.co.id/api/auth/validate-password


B. Response Data
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
=================== ==============================


C. Examples
-------------------

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


2. Set PIN
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
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>"  https://stamps.co.id/api/pin/set -i -d '{ "user": 123, "pin": "123456", "confirm_pin": "123456" }'

B. Response Data
----------------

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
        "error_code": "pin_mismatch",
        "error_message": "confirm_pin: Confirmation PIN does not match"
    }


3. Change PIN
=============
| URL endpoint: https://stamps.co.id/api/pin/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=============== ======== ===========
Parameter       Required Description
=============== ======== ===========
user            Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
current_pin     Yes      Customer's previously set 6 digit string PIN
new_pin         Yes      6 digit string
confirm_new_pin Yes      6 digit string that needs to be the same as ``new_pin`` parameter
=============== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>"  https://stamps.co.id/api/pin/change -i -d '{ "user": 123, "current_pin": "123456", "new_pin": "654321", "confirm_new_pin", "654321" }'

B. Response Data
----------------

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
        "error_code": "pin_mismatch",
        "error_message":"confirm_new_pin: Confirmation PIN does not match"
    }


4. Validate PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/validate
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Customer's PIN will be blocked in case of repeated failed validation. Failures count will be reset 604800 seconds (1 week) after the last failure.

========= ======== ===========
Parameter Required Description
========= ======== ===========
user      Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
pin       Yes      6 digit string
========= ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/pin/validate -i -d '{ "user": 123, "pin": "123456" }'

B. Response Data
----------------

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
        "detail": "pin: Invalid PIN, 2 attempt(s) left",
        "errors": {
            "pin": "Invalid PIN, 2 attempt(s) left"
        },
        "error_code": "invalid_pin",
        "error_message": "pin: Invalid PIN, 2 attempt(s) left"
    }


5. Unblock PIN
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
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/pin/unblock -i -d '{ "user": 123 }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }


6. Requesting an OTP to Reset PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/request-otp-for-reset
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Request an OTP to reset customer's PIN. OTP will be send to customer's email or mobile phone if ``template_code`` parameter is provided.

============= ======== ===========
Parameter     Required Description
============= ======== ===========
user          Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
type          No       A string for OTP sending method choice, supports ``email``, ``sms``, and ``whatsapp``
template_code No       A string indicating the template to be used to send the OTP
============= ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/pin/request-otp-for-reset -i -d '{ "user": 123 }'

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
otp                 6 digit string
=================== ==============================

C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST
    [Redacted Header]

    {
        "status": "ok",
        "otp": "123456"
    }


7. Reset PIN
===============
| URL endpoint: https://stamps.co.id/api/pin/reset
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
otp         Yes      6 digit string OTP received from ``Requesting an OTP to Reset PIN`` API
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/pin/reset -i -d '{ "user": 123, "otp": "123123", "pin": "654321", "confirm_pin", "654321" }'

B. Response Data
----------------

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
        "error_code": "pin_mismatch",
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


8. Reset PIN with Password
==========================
| URL endpoint: https://stamps.co.id/api/pin/reset-with-password
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

=========== ======== ===========
Parameter   Required Description
=========== ======== ===========
user        Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
password    Yes      User's password
pin         Yes      6 digit string
confirm_pin Yes      6 digit string that needs to be the same as ``pin`` parameter
=========== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/pin/reset-with-password -i -d '{ "user": 123, "password": "secret123", "pin": "654321", "confirm_pin", "654321" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Invalid password:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "password: Invalid user password",
        "error_code": "invalid_password",
        "error_message": "password: Invalid user password",
        "errors": {
            "password": "Invalid user password"
        }
    }


9. Change Password
===============
| URL endpoint: https://stamps.co.id/api/auth/change-password
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

====================== ======== ===========
Parameter              Required Description
====================== ======== ===========
user                   Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
old_password           Yes      Customer's current password
new_password           Yes      New password
====================== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/change-password -i -d '{ "user": "test@gmail.com", "old_password": "secure_password", "new_password": "new_secure_password" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }


10. Set Password
===============
| URL endpoint: https://stamps.co.id/api/auth/set-password
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

====================== ======== ===========
Parameter              Required Description
====================== ======== ===========
email                  Yes      Customer's email
new_password           Yes      New password
====================== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/set-password -i -d '{ "email": "foo@bar.com", "new_password": "secure_password" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "id": 1,
        "name": "Foo",
        "email": "foo@bar.com",
        "status": "ok"
    }


11. Requesting an OTP to Reset Password
===============
| URL endpoint: https://stamps.co.id/api/auth/request-otp-for-password-reset
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

Request an OTP to reset customer's password. OTP will be send to customer's email or mobile phone if ``template_code`` parameter is provided.

============= ======== ===========
Parameter     Required Description
============= ======== ===========
identifier    Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
type          Yes      A string for OTP sending method choice, supports ``email``, ``sms``, and ``whatsapp``
template_code No       A string indicating the template to be used to send the OTP
============= ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/request-otp-for-password-reset -i -d '{ "identifier": 123, "type": "email" }'

B. Response Data
----------------

=================== ==============================
Variable            Description
=================== ==============================
status              Returns ``ok`` if successful
otp                 6 digit string
=================== ==============================

C. Examples
-----------

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST
    [Redacted Header]

    {
        "status": "ok",
        "otp": "123456"
    }


12. Reset Password with OTP
===============
| URL endpoint: https://stamps.co.id/api/auth/reset-password-with-otp
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

==================== ======== ===========
Parameter            Required Description
==================== ======== ===========
identifier           Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
otp                  Yes      6 digit string OTP received from ``Requesting an OTP to Reset Password`` API
new_password         Yes      A secure password
confirm_new_password Yes      A secure password that needs to be the same as ``new_password`` parameter
==================== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/reset-password-with-otp -i -d '{ "identifier": 123, "otp": "123123", "new_password": "securepassword123", "confirm_new_password", "securepassword123" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Mismatch ``new_password`` and ``confirm_new_password`` parameter:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "detail": "confirm_new_password: Confirmation password does not match",
        "errors": {
            "confirm_new_password": "Confirmation password does not match"
        },
        "error_code": "mismatch_password",
        "error_message": "confirm_new_password: Confirmation password does not match"
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


13. Validate Mobile Number
===============
| URL endpoint: https://stamps.co.id/api/auth/validate-mobile-number
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

==================== ======== ===========
Parameter            Required Description
==================== ======== ===========
mobile_number        Yes      A string indicating mobile number or phone to validate
==================== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/validate-mobile-number -i -d '{ "mobile_number": "081234567890" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Invalid Mobile Number:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "status": "invalid",
        "detail": "mobile_number: Please enter a valid mobile phone number.",
        "errors": {
            "mobile_number": "Please enter a valid mobile phone number."
        },
        "error_code": "invalid_mobile_number",
        "error_message": "mobile_number: Please enter a valid mobile phone number."
    }


14. Reset OTP Limit
===============
| URL endpoint: https://stamps.co.id/api/auth/reset-otp-limit
| Allowed Method: POST
| Require Authentication: Yes

A. Request
----------

==================== ======== ===========
Parameter            Required Description
==================== ======== ===========
user                 Yes      A string indicating customer's email, Member ID, mobile number or primary key ID
==================== ======== ===========

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/auth/reset-otp-limit -i -d '{ "user": "081234567890" }'

B. Response Data
----------------

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
    Allow: POST
    [Redacted Header]

    {
        "status": "ok"
    }

Invalid User:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    [Redacted Header]

    {
        "status": "invalid",
        "detail": "user: Please enter a valid mobile phone number.",
        "errors": {
            "user": "Please enter a valid mobile phone number."
        },
        "error_code": "invalid_user",
        "error_message": "user: Please enter a valid mobile phone number."
    }
