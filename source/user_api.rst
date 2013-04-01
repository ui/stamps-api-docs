************************************
User API
************************************

1. Querying for total stamps amount
=======================================
| URL endpoint: https://stamps.co.id/api/users/stamp
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
    user_email  Yes         A string indicating user’s
                            email address to be queried
    =========== =========== =========================

    Here’s an example of how the API call might look like in JSON format::

        {
            "token": “aaaabbbbccccddddeeeefffff”,
            "user_email": “Customer@stamps.co.id”,
        }

    Example of API call request using cURL::

    $ curl –X POST –H “Content-Type: application/json” –d ‘{ “token”: “aaabbbcccdddeeefff”, “user_email”: “Customer@stamps.co.id”}’ https://stamps.co.id/api/users/add 

B. Response
-----------------------------
    In response to this API call, Stamps will return response with the following data (in JSON):

    =================== ==============================
    Variable            Description
    =================== ==============================
    stamps              Total stamps amount the
                        particular user has 
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
                        wrong on Stamps’ end
    =================== ==============================

    Here’s an example on how the Stamps API will response to the call

    **If query is successful:** ::

        HTTP/1.0 200 OK
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {“stamps”: 70}

    **If transaction is unsuccessful (often missing parameters):** ::

        HTTP/1.0 400 BAD REQUEST
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {“detail”: “Your transaction cannot be completed due to the following error(s)”, “errors”: [{“reward”: “This field is required”}]}

    **If using http:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {“detail”: “Please use https instead of http”}


    **If missing or wrong authentication token:** ::

        HTTP/1.0 403 FORBIDDEN
        Vary: Accept
        Content-Type: application/json
        Allow: POST, OPTIONS
         [Redacted Header]

        {“detail”: “Authentication credentials were not provided.”}