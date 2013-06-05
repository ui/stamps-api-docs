************************************
Redemption API
************************************

1. Adding new redemption
=============================
| URL endpoint: https://stamps.co.id/api/redemptions/add
| Allowed Method: POST
| Require Authentication: Yes
| Expected content type: application/json

A. Request
-----------------------------
    You can add a new transaction on stamps by calling the API with these parameters

    =========== =========== =========================
    Parameter   Required    Description
    =========== =========== =========================
    token       Yes         Authentication string
    user_email  Yes         A string indicating user's
                            email address
    store       Yes         A number indicating
                            Merchant's store id where the transaction is initiated
    reward      Yes         A number indicating the
                            reward's ID
    =========== =========== =========================

Here's an example of how the API call might look like in JSON format::

        {
            "token": "aaaabbbbccccddddeeeefffff",
            "user_email": "Customer@stamps.co.id",
            "store": 32,
            "reward": 1
        }

Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -d '{ "token": "aaabbbcccdddeeefff", "user_email": "Customer@stamps.co.id", "store": 32, "reward": 12}' https://stamps.co.id/api/redemptions/add

B. Response
-----------------------------

In response to this API call, Stamps will return response with the following data (in JSON):

    =================== ==============================
    Variable            Description
    =================== ==============================
    redemption          Redemption information which is
                        successfully created.
                        Contains id, reward, and stamps_used
    customer            Customer information after successful
                        redemption. Contains id and stamps_remaining.

    detail              Description of error (if any)
    validation_errors   Errors encountered when parsing
                        data (if any)
    =================== ==============================

Depending on the request, responses may return these status codes:

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

    **If transaction is successful:** ::

        HTTP/1.0 200 OK
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {
          "customer": {
            "id": 6,
            "stamps_remaining": 60
          },
          "redemption": {
            "reward": "Free Scoop of Ice Cream",
            "id": 1,
            "stamps_used": 10
          }
        }


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
