************************************
Messages API
************************************

1. Send Message
===============
| URL endpoint: https://stamps.co.id/api/messages/send
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to send a templated message. A message template can be an SMS or WhatsApp message.

============= =========== =========================
Parameter     Required    Description
============= =========== =========================
template      Yes         String indicating the message template code
recipient     Yes         Target mobile number for the message
context       Yes         JSON object specifying context for message template
============= =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: <token_type> <token>" https://stamps.co.id/api/messages/send -i -d '{ "template": "otp_wa", "recipient": "+628123456789", "context": {"otp": "123123"}}'


B. Response
----------------
Please see :ref:`response-codes` for a complete list of API response codes.

On a successful API call, Stamps will respond with the following data:

=================== ==============================
Variable            Description
=================== ==============================
status              status
=================== ==============================


C. Examples
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
