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


=========================== =========== =======================
Parameter                   Required    Description
=========================== =========== =======================
token                       Yes         Authentication string
user                        No          Email address / Member ID indicating customer.
                                        Leaving this empty creates an ``open`` transaction.
store                       Yes         A number (id) indicating store where transaction
                                        is created
invoice_number              Yes         POS transaction number (must be unique daily)
total_value                 Yes         A number indicating transaction's grand total
number_of_people            Yes         An integer indicating the number of people involved in transaction
created                     Yes         ISO 8601 date time format to indicate transaction's
                                        created date
                                        (e.g. 2013-02-15T13:01:01+07)
sub_total                   No          A number indicating transaction subtotal
discount                    No          A number indicating transaction discount (in Rp.)
service_charge              No          A number indicating service charge (in Rp.)
tax                         No          A number indicating transaction tax (in Rp.)
channel                     No          Channel of a transaction, for channel mapping, see table below
type                        No          The type of prepared transactions, for type mapping, see table below
items                       No          List of items containing product name, quantity, subtotal &
                                        stamps_subtotal (optional).
                                        ``price`` is the combined price of products (qty * unit price),
                                        ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps),
                                        this field is optional.
payments                    No          List of payments object containing value, payment_method, and
                                        eligible_for_membership(optional).
                                        ``value`` is the amount of payment
                                        ``payment_method`` is the method used for payment, for payment method mapping, see table below
                                        ``eligible_for_membership`` whether this payment is used for member's status/level changes.
                                        This field is optional. Default to true if not provided(can be configured later).
stamps                      No          A number indicating custom stamps
require_email_notification  No          A boolean indicating send transaction to email if customer can retrieve email
employee_code               No          Employee code of sender employee
extra_data                  No          Additional data for further processing
=========================== =========== =======================

Channel Mapping

=================== ===========
Code                Description   
=================== ===========
1                   Mobile App
2                   POS
3                   Kiosk
4                   Web
5                   Android
6                   iOS
7                   Call Center
8                   GrabFood
9                   GoFood
=================== ===========



Type Mapping

=================== ===========
Code                Description   
=================== ===========
1                   Delivery
2                   Dine-in
3                   Take out
4                   E-Commerce
5                   Pickup
=================== ===========



Payment Method Mapping

=================== ===========
Code                Description   
=================== ===========
10                  Cash
11                  Exact Cash
12                  Rp50000
13                  Rp100000
20                  Debit Card
30                  Credit Card
31                  BCA Credit Card
32                  CIMB Credit Card
40                  Digital
41                  GO-PAY
42                  Dana
43                  OVO
44                  Link Aja
45                  BCA Virtual Account
46                  Shopee Pay
47                  Sakuku
50                  Voucher
51                  MAP 50000
52                  MAP 100000
53                  MAP 500000
54                  SOGO 25000
55                  SOGO 50000
56                  SOGO 10000
57                  SODEXO
58                  GL 50000
59                  GL 100000
60                  Others
61                  VMM Mandiri
62                  BCA Debit
63                  BCA Card
64                  Flazz
65                  GO-RESTO
66                  DANA Online
67                  GRAB-IM
68                  OVO Online
69                  GO-PAY Online
70                  Shopee Online
71                  BNI Card
72                  BNI
73                  Traveloka
=================== ===========



Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "user": "customer@stamps.co.id",
       "stamps": 10,
       "store": 32,
       "invoice_number": "my_invoice_number",
       "sub_total": 45000,
       "total_value": 50000,
       "number_of_people": 8,
       "tax": 5000,
       "channel": 1,
       "require_email_notification": False,
       "employee_code": "12345",
       "type": 2,
       "created": "2013-02-15T13:01:01+07",
       "extra_data": {
          "employee_name": "Stamps Employee",
          "order_number": "123456"
       }
       "items": [
          {
             "product_name": "Cappucino",
             "quantity": 2,
             "subtotal": 10000,
             "stamps_subtotal": 4
          },
          {
             "product_name": "Iced Tea",
             "quantity": 4,
             "subtotal": 5000,
             "stamps_subtotal": 4
          }
       ],
       "payments": [
          {
            "value": 30000,
            "payment_method": 10
          },
          {
            "value": 20000,
            "payment_method": 43,
            "eligible_for_membership": false
          }
       ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/v2/transactions/add -i -d '{ "token": "secret", "created": "2017-03-30T07:01:01+07", "user": "customer@stamps.co.id", "store": 422, "number_of_people": 8, "tax":5000, "channel":1, "type":2, "invoice_number": "invoice_1", "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "subtotal": 10000}, {"product_name": "Iced Tea", "quantity": 4, "subtotal": 5000}]}, "payments": [{"value": 30000, "payment_method": 10}, {"value": 20000, "payment_method": 43, "eligible_for_membership": false}]'

B. Response
-----------------------------

In response to this API call, Stamps will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
transaction         Stamps transaction information
                    that is successfully created.
                    Contains id, value, number_of_people, discount and stamps_earned.
customer            Customer information after successful
                    transaction. Contains id, mobile_phone, stamps_remaining, balance and status.
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
        "number_of_people": 8,
        "discount": 5000.0
      }
    }


When some fields don't validate (JSON):

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]


    {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"subtotal": "This field is required."}, {"invoice_number": "Store does not exist"}]}


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
 
3. Modify Transaction's Value or Items
=============================
| URL endpoint: https://stamps.co.id/api/v2/transactions/modify
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can modify transaction's value or items detail on stamps by calling the API with these parameters


========================== =========== =========================================================
Parameter                  Required    Description
========================== =========== =========================================================
token                      Yes         Authentication string
id                         Yes         Transaction ID
total_value                Yes         Total value that want to deduct from a transaction
subtotal                   Yes         Sub total value that want to deduct from a transaction
items                      Yes         Items detail that want to deduct from a transaction
========================== =========== =========================================================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "id": 1,
       "total_value": -4000,
       "subtotal": -3000,
       "items": [
            {
                "product_name": "AQUA",
                "quantity": -1
            }
        ]
    }


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/transactions/cancel -i -d '{ "token": "secret", "id": 1, "total_value": -4000,"subtotal": -3000,"items": [{"product_name": "AQUA","quantity": -1}]'


B. Response
-----------------------------

In response to this API call, Stamps will return response with the following data (in JSON by default):

=================== ==================
Variable            Description
=================== ==================
transaction         Transaction information which is
                    successfully modified.
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
        "id": 1,
        "value": 30000,
        "stamps_earned": 3,
        "number_of_people": 1
      },
      "customer": {
        "id": 5,
        "mobile_phone":null,
        "stamps_remaining": 62,
        "status": "Blue",
        "balance":0       
      }
    }


When some fields don't validate:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"detail":"product_name: Product does not exists","error_message":"product_name: Product does not exists","error_code":"product_not_found","errors":{"product_name":"Product does not exists"}}



4. Getting Transaction Detail
=============================
| URL endpoint: https://stamps.co.id/api/transactions/details
| Allowed method: GET
| Requires authentication: Yes


A. Request
-----------------------------

You can get transaction's detail data through this API.

========================== =========== =========================================================
Parameter                  Required    Description
========================== =========== =========================================================
token                      Yes         Authentication string
transaction_id             Yes         Transaction ID
merchant                   Yes         Total value that want to deduct from a transaction
========================== =========== =========================================================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/transactions/details?token=abc&merchant=123&transaction_id=345'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
transaction         An object containing transaction information after successful request.
=================== ==============================


C. Example Response
-------------------

On successful balance update:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

      {
        "transaction": {
            "created": 1619734844,
            "discount": null,
            "items": [{
                  "id": 1,
                  "name": "Cafe Latte",
                  "quantity": 1.0,
              },
              {
                  "id": 2,
                  "name": "Fried Rice",
                  "quantity": 1.0,
              }
            ],
            "notes": "",
            "service_charge": null,
            "stamps": 150,
            "status": "Created",
            "store": {
                "display_name": "My Favorite Store",
                "id": 1,
                "name": "Fav Store"
            },
            "subtotal": null,
            "tax": null,
            "type": null,
            "value": 1500000.0
        }
    }

  
