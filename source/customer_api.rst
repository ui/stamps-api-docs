************************************
User API
************************************

1. Querying for customer data
=======================================
| URL endpoint: https://stamps.co.id/api/memberships/status
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for a customer's data on Stamps .

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user_email  Yes         A string indicating user's
                        email address to be queried
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    # Please note that for cURL command you need to escape special characters
    $ curl 'https://stamps.co.id/api/memberships/status?token=abc&user_email=customer@stamps.co.id'

B. Response
-----------------------------
    In response to this API call, Stamps will return response with the following data (in JSON):

    =================== ==============================
    Variable            Description
    =================== ==============================
    stamps              Total stamps amount the
                        particular user has
    membership_status   Membership status of the user
    detail              Description of error (if any)
    validation_errors   Errors encountered when parsing
                        data (if any)
    =================== ==============================

    With these possible HTTP headers:

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

    Here's an example on how the Stamps API will response to the call

    **If query is successful:** ::

        HTTP/1.0 200 OK
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"stamps": 8917, "membership_status": "Gold"}

    **If transaction is unsuccessful (often missing parameters):** ::

        HTTP/1.0 400 BAD REQUEST
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"reward": "This field is required"}]}

    **If using http:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Please use https instead of http"}


    **If missing or wrong authentication token:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Authentication credentials were not provided."}


2. Querying for customer's eligible rewards
=======================================
| URL endpoint: https://stamps.co.id/api/memberships/eligible-rewards
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for a customer's eligible rewards on Stamps .

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user_email  Yes         A string indicating user's
                        email address to be queried
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    # Please note that for cURL command you need to escape special characters
    $ curl 'https://stamps.co.id/api/memberships/eligible-rewards?token=abc&user_email=customer@stamps.co.id'

B. Response
-----------------------------
    In response to this API call, Stamps will return response with the following data (in JSON):

    =================== ==============================
    Variable            Description
    =================== ==============================
    eligible rewards    List of rewards which customer
                        is eligible to redeem. Contain
                        name, stamps_required, and image_url
    detail              Description of error (if any)
    validation_errors   Errors encountered when parsing
                        data (if any)
    =================== ==============================

    With these possible HTTP headers:

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

    Here's an example on how the Stamps API will response to the call

    **If query is successful:** ::

        HTTP/1.0 200 OK
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"eligible_rewards": [{"name": "Kopi Tarik", "stamps_required": 50, "image_url": "http://foo.com"}, {"name": "Teh Tarik", "stamps_required": 20}, "image_url": "http://foo.com"]}

    **If transaction is unsuccessful (often missing parameters):** ::

        HTTP/1.0 400 BAD REQUEST
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"reward": "This field is required"}]}

    **If using http:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Please use https instead of http"}


    **If missing or wrong authentication token:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {"detail": "Authentication credentials were not provided."}
