************************************
Feedback API
************************************

1. Adding a Feedback
=======================
| URL endpoint: https://stamps.co.id/api/feedbacks/add
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can add a new transaction on Stamps by calling the API with these parameters


=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
token               Yes         Authentication string
user                Yes         Email address / Member ID indicating customer
score               No          Integer (1 to 5) representing how happy the customer is with his experience.
                                This field is required if content is left blank.
content             No          Text content representing customer's comments or feedback.
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "user": "customer@stamps.co.id",
       "score": 5,
       "content": "Very happy with Stamps!"
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/transactions/add -i -d '{ "token": "secret", "user": "customer@stamps.co.id", "score": 5, "content": "Very happy with Stamps!"}'


B. Response
-----------------------------

In response to this API call, Stamps will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
feedback            Feedback that's successfully created
                    Contains id, content and score.
detail              Description of error (if any)
validation_errors   Errors encountered when parsing data (if any)
=================== ==================

Depending on the request, responses may return these status codes:

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request, usually missing a required parameter
401                 Unauthorized, usually missing or wrong authentication token
403                 Forbidden – You do not have permission for this request
405                 HTTP method not allowed
500, 502, 503, 504  Server errors - something is wrong on Stamps' end
=================== ==============================

Below are a few examples responses on successful API calls.


If transaction is successful(JSON):

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "feedback": {
        "id": 17,
        "score": 5,
        "content": "Very happy with Stamps!"
      }
    }
