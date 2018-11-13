************************************
Transaction API
************************************

1. Adding a Transaction
=======================
| URL endpoint: https://stamps.co.id/api/v2/transactions/add
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can add a new transaction on Stamps by calling the API with these parameters


=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
token               Yes         Authentication string
user                No          Email address / Member ID indicating customer.
                                Leaving this empty creates an ``open`` transaction.
store               Yes         A number (id) indicating store where transaction
                                is created
invoice_number      Yes         POS transaction number (must be unique daily)
total_value         Yes         A number indicating transaction's grand total
number_of_people    Yes         An integer indicating the number of people involved in transaction
created             Yes         ISO 8601 date time format to indicate transaction's
                                created date
                                (e.g. 2013-02-15T13:01:01+07)
sub_total           No          A number indicating transaction subtotal
discount            No          A number indicating transaction discount (in Rp.)
service_charge      No          A number indicating service charge (in Rp.)
tax                 No          A number indicating transaction tax (in Rp.)
items               No          List of items containing product name, quantity, price &
                                stamps_subtotal.
                                ``price`` is the combined price of products (qty * unit price),
                                ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps).
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "user": "customer@stamps.co.id",
       "store": 32,
       "invoice_number": "secret123456",
       "total_value": 50000,
       "number_of_people": 8,
       "tax": 5000,
       "created": "2013-02-15T13:01:01+07",
       "items": [
          {
             "product_name": "Cappucino",
             "quantity": 2,
             "price": 10000,
             "stamps_subtotal": 4
          },
          {
             "product_name": "Iced Tea",
             "quantity": 4,
             "price": 5000,
             "stamps_subtotal": 4
          }
       ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/v2/transactions/add -i -d '{ "token": "secret", "created": "2017-03-30T07:01:01+07", "user": "customer@stamps.co.id", "store": 422, "number_of_people": 8, "invoice_number": "invoice_1", "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "price": 10000}, {"product_name": "Iced Tea", "quantity": 4, "price": 5000}]}'

B. Response
-----------------------------

In response to this API call, Stamps will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
transaction         Stamps transaction information
                    that is successfully created.
                    Contains id, value, and stamps_earned.
customer            Customer information after successful
                    transaction. Contains id, stamps_remaining, balance and status.
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
500, 502, 503, 504  Something went wrong on Stamps' server
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
        "balance": 150000,
        "mobile_phone": "+6281314811365",
        "id": 8120,
        "stamps_remaining": 401
      },
      "transaction": {
        "stamps_earned": 5,
        "id": 2374815,
        "value": 50000.0,
        "number_of_people": 8
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


C. Legacy Endpoint
------------------
Legacy endpoint's documentation is available at `Legacy transaction API <http://docs.stamps.co.id/en/latest/legacy_transaction_api.html>`_



2. Canceling a Transaction
=============================
| URL endpoint: https://stamps.co.id/api/transactions/cancel
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can cancel a transaction on stamps by calling the API with these parameters


========================== =========== =========================================================
Parameter                  Required    Description
========================== =========== =========================================================
token                      Yes         Authentication string
id                         Yes         Transaction ID
cancel_related_redemptions No          When "true", cancels all redemptions registered in under
                                       this transaction's "invoice_number". Defaults to "false"
========================== =========== =========================================================


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
500, 502, 503, 504  Something went wrong on Stamps' server
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
