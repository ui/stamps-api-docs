************************************
Transaction API
************************************

1. Adding a Transaction
=======================
| URL endpoint: https://stamps.co.id/api/transactions/add
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can add a new transaction on stamps by calling the API with these parameters


=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
token               Yes         Authentication string
user                Yes         Email address / Member ID indicating customer
store               Yes         A number (id) indicating store where transaction
                                is created
invoice_number      Yes         POS transaction number (must be unique daily)
total_value         Yes         A number indicating transaction's grand total
created             No          ISO 8601 date time format to indicate transaction's
                                created date
                                (e.g. 2013-02-15T13:01:01+07)
subtotal            No          A number indicating transaction subtotal
discount            No          A number indicating transaction discount (in Rp.)
service_change      No          A number indicating service charge (in Rp.)
tax                 No          A number indicating transaction tax (in Rp.)
items               No          List of items containing product name, price & qty
secondary_merchant  No          A merchant id to attach this transaction to
secondary_store     No          If specified, transaction will be assigned to this secondary merchant's store
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "user": "customer@stamps.co.id",
       "store": 32,
       "invoice_number": "secret123456",
       "total_value": 50000,
       "subtotal": 40000,
       "discount": 0,
       "service_charge": 5000,
       "tax": 5000,
       "items": [
          {
             "product_name": "Cappucino",
             "quantity": 2,
             "price": 10000
          },
          {
             "product_name": "Iced Tea",
             "quantity": 4,
             "price": 5000
          }
       ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/transactions/add -i -d '{ "token": "secret", "user": "customer@stamps.co.id", "store": 2, "invoice_number": "invoice_1", "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "price": 10000}, {"product_name": "Iced Tea", "quantity": 4, "price": 5000}]}'


B. Response
-----------------------------

In response to this API call, Stamps will return response with the following data (in JSON by default):

    =================== ==================
    Variable            Description
    =================== ==================
    transaction         Stamps transaction information
                        that is successfully created.
                        Contains id, value, and stamps_earned.
    customer            Customer information after successful
                        transaction. Contains id, stamps_remaining, and status.
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
      "customer": {
        "status": "Blue",
        "id": 17,
        "stamps_remaining": 11
      },
      "transaction": {
        "stamps_earned": 1,
        "id": 2,
        "value": 15000
      }
    }


When some fields don't validate (JSON):

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]


    {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"price": "This field is required."}, {"invoice_number": "Store does not exist"}]}


If HTTP is used instead of HTTPS:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"detail": "Please use https instead of http"}


If missing or wrong authentication token:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"detail": "Authentication credentials were not provided."}



2. Canceling a Transaction
=============================
| URL endpoint: https://stamps.co.id/api/transactions/cancel
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can cancel a transaction on stamps by calling the API with these parameters


=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
token               Yes         Authentication string
id                  Yes         Transaction ID
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "id": 1
    }


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/transactions/cancel -i -d '{ "token": "secret", "id": 1 }'


B. Response
-----------------------------

In response to this API call, Stamps will return response with the following data (in JSON by default):

=================== ==================
Variable            Description
=================== ==================
transaction         Transaction information which is
                    successfully canceled.
                    Contains stamps_earned, id, and value
customer            Customer information after successful
                    redemption. Contains id, status, and stamps_remaining.
errors              Errors encountered when canceling a transaction (if any)
=================== ==================

C. Response Headers
-------------------

Depending on the request, responses may return these status codes:

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request, usually missing a required parameter
401                 Unauthorized, usually missing or wrong authentication token
403                 Forbidden – You do not have permission for this request
404                 Cannot find transaction of the requested transaction id
405                 HTTP method not allowed
500, 502, 503, 504  Server errors - something is wrong on Stamps' end
=================== ==============================

D. Example Response
-------------------

Below are a few examples responses on successful API calls.


If transaction is successfully canceled:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "transaction": {
        "stamps_earned": 3,
        "id": 1,
        "value": 30000
        "status": "Canceled"
      },
      "customer": {
        "status": "Blue",
        "id": 5,
        "stamps_remaining": 62
      }
    }


When some fields don't validate:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"errors": {"info": "Transaction can't be canceled due to insufficient Stamps"}}
