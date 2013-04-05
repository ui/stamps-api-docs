************************************
Transaction API
************************************

1. Adding new transaction
=============================
| URL endpoint: https://stamps.co.id/api/transactions/add
| Allowed Method: POST
| Require Authentication: Yes
| Expected content type: application/json


A. Request
-----------------------------
    You can add a new transaction on stamps by calling the API with these parameters


    =============== =========== =======================
    Parameter       Required    Description
    =============== =========== =======================
    token           Yes         Authentication string
    user_email      Yes         A string indicating user's
                                email address
    store           Yes         A number indicating Merchant's
                                store id where the transaction is initiated
    invoice_number  Yes         POS transaction number (must
                                be unique daily)
    total_value     Yes         A number indicating
                                transaction's grand total
    subtotal        Optional    A number indicating
                                transaction subtotal
    discount        Optional    A number indicating
                                transaction discount (in Rp.)
    service_change  Optional    A number indicating
                                transaction service charge (in Rp.)
    tax             Optional    A number indicating
                                transaction tax (in Rp.)
    items           Optional    List of items containing
                                product_name, price and qty
    =============== =========== =======================

    Here's an example of how the API call might look like in JSON format::

.. code-block:: javascript

        {
           "token": "aaaabbbbccccddddeeeefffff",
           "user_email": "customer@stamps.co.id",
           "store": 32,
           "invoice_number": "abc123456",
           "total_value": 50000,
           "subtotal": 40000,
           "discount": 0,
           "service_charge": 5000,
           "tax": 5000,
           "items": [
              {
                 "product_name": "Cappucino",
                 "quantity": 2,
                 "price": 10000,
              },
              {
                 "product_name": "Iced Tea",
                 "quantity": 4,
                 "price": 5000,
              }
           ]
        }

    Example of API call request using cURL

.. code-block :: bash

    $ curl –X POST –H "Content-Type: application/json" –d '{ "token": "aaabbbcccdddeeefff", "user_email": "Customer@stamps.co.id", "store": 32, "invoice_number": "abc123456", "total_value": 50000, "subtotal": 40000, "discount": 0, "service_charge": 5000, "tax": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "price": 10000}, {"product_name": "Iced Tea", "quantity": 4, "price": 5000]' https://stamps.co.id/api/transaction/add


B. Response
-----------------------------
    In response to this API call, Stamps will return response with the following data (in JSON):

    =================== ==================
    Variable            Description
    =================== ==================
    transaction_id      Stamps transaction ID that is successfully created
    detail              Description of error (if any)
    validation_errors   Errors encountered when parsing data (if any)
    =================== ==================

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
    405                 HTTP method not allowed - the
                        requested resources cannot be called with the specified HTTP method
    500, 502, 503, 504  Server Errors - something is
                        wrong on Stamps' end
    =================== ==============================

    Below are a few examples responses on successful API calls.

    If transaction is successful:

.. code-block :: bash

      HTTP/1.0 200 OK
      Vary: Accept
      Content-Type: application/json
      Allow: POST, OPTIONS
      [Redacted Header]
      {“transaction_id”: 3513}


    When some fields don't validate:

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
