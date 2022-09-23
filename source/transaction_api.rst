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
items                       No          List of items containing product name, quantity, subtotal,
                                        stamps_subtotal (optional) & eligible_for_stamps (optional).
                                        ``price`` is the combined price of products (qty * unit price),
                                        ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps),
                                        this field is optional.
                                        ``eligible_for_stamps`` is boolean value to determine whether the item should be included in Stamps Calculation. Defaults to ``true``.
payments                    No          List of payments object containing value, payment_method, and
                                        eligible_for_membership(optional).
                                        ``value`` is the amount of payment
                                        ``payment_method`` is the method used for payment
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
       "employee_code": "employee_code",
       "type": 2,
       "created": "2013-02-15T13:01:01+07",
       "extra_data": {
          "employee_name": "Stamps Employee",
          "order_number": "order_number"
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
             "stamps_subtotal": 4,
             "eligible_for_stamps": False
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


1. Adding a Transaction with Redemptions
=======================
| URL endpoint: https://stamps.co.id/api/v2/transactions/add-with-redemptions
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can add a new transaction with redemptions on Stamps by calling the API with these parameters


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
items                       No          List of items containing product name, quantity, subtotal,
                                        stamps_subtotal (optional) & eligible_for_stamps (optional).
                                        ``price`` is the combined price of products (qty * unit price),
                                        ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps),
                                        this field is optional.
                                        ``eligible_for_stamps`` is boolean value to determine whether the item should be included in Stamps Calculation. Defaults to ``true``.
payments                    No          List of payments object containing value, payment_method, and
                                        eligible_for_membership(optional).
                                        ``value`` is the amount of payment
                                        ``payment_method`` is the method used for payment
                                        ``eligible_for_membership`` whether this payment is used for member's status/level changes.
                                        This field is optional. Default to true if not provided(can be configured later).
stamps                      No          A number indicating custom stamps
require_email_notification  No          A boolean indicating send transaction to email if customer can retrieve email
employee_code               No          Employee code of sender employee
extra_data                  No          Additional data for further processing
reward_redemptions          No          List of reward objects that want to be redeemed. Contains ``request_id``, ``reward``, and ``stamps`` (required if reward type is flexible reward). ``reward`` field can be filled with either reward ID (integer, i.e. ``1``) or reward code (string, i.e. ``REWARD1``)
voucher_redemptions         No          List of voucher objects that want to be redeemed. Contains ``request_id`` and ``voucher_code``
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
       "employee_code": "employee_code",
       "type": 2,
       "created": "2013-02-15T13:01:01+07",
       "extra_data": {
          "employee_name": "Stamps Employee",
          "order_number": "order_number"
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
             "stamps_subtotal": 4,
             "eligible_for_stamps": False
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
       ],
       "reward_redemptions": [
          {
            "request_id": "request-id-1",
            "reward": 1
          },
          {
            "request_id": "request-id-1",
            "reward": "REWARDCODE"
          },
          {
            "request_id": "request-id-1",
            "reward": 1,
            "stamps": 10,
          }
          {
            "request_id": "request-id-1",
            "reward": "REWARDCODE",
            "stamps": 10,
          }
       ],
       "voucher_redemptions": [
          {
            "request_id": "request-id-1",
            "voucher_code": "VOUCHERCODE"
          }
       ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/v2/transactions/add-with-redemptions -i -d '{ "token": "secret", "created": "2017-03-30T07:01:01+07", "user": "customer@stamps.co.id", "store": 422, "number_of_people": 8, "tax":5000, "channel":1, "type":2, "invoice_number": "invoice_1", "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "subtotal": 10000}, {"product_name": "Iced Tea", "quantity": 4, "subtotal": 5000}]}, "payments": [{"value": 30000, "payment_method": 10}, {"value": 20000, "payment_method": 43, "eligible_for_membership": false}], "reward_redemptions": [ { "request_id": "request-id-1", "reward": 1 }, { "request_id": "request-id-1", "reward": "REWARDCODE" }, { "request_id": "request-id-1", "reward": 1, "stamps": 10, } { "request_id": "request-id-1", "reward": "REWARDCODE", "stamps": 10, } ], "voucher_redemptions": [ { "request_id": "request-id-1", "voucher_code": "VOUCHERCODE" } ]'

B. Response
-----------------------------

In response to this API call, Stamps will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
transaction         Stamps transaction information
                    that is successfully created.
                    Contains id, value, number_of_people, discount and stamps_earned.
membership          Contains membership data.
                    Contains ``tags``, ``status``, ``status_text``, ``stamps``, ``balance``,
                    ``is_blocked``, ``referral_code``, ``start_date``, and ``created``
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
      "membership": {
        "tags": [],
        "status": 10,
        "status_text": "Blue",
        "stamps": 10,
        "balance": 20,
        "is_blocked": false,
        "referral_code": "asd",
        "start_date": "2020-01-01",
        "created": "2020-01-01",
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



1. Canceling a Transaction
=============================
| URL endpoint: https://stamps.co.id/api/v2/transactions/cancel
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

You can cancel a transaction on Stamps by calling the API with these parameters


========================== =========== =========================================================
Parameter                  Required    Description
========================== =========== =========================================================
token                      Yes         Authentication string
invoice_number             Yes         The transaction's invoice number
date                       Yes         Date when the transaction is executed, in YYYY-MM-DD format
========================== =========== =========================================================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "invoice_number": "ABCD123",
       "date": "2021-02-01"
    }


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/v2/transactions/cancel -i -d '{ "token": "secret", "invoice_number": "ABCD123", "date": "2021-02-01" }'


B. Response
-----------------------------

In response to this API call, Stamps will return response with the following data (in JSON by default):

=================== ==================
Variable            Description
=================== ==================
transaction         Transaction information which is
                    successfully canceled.
                    Contains stamps_earned, id, and value
membership          Contains membership data.
                    Contains ``tags``, ``status``, ``status_text``, ``stamps``, ``balance``,
                    ``is_blocked``, ``referral_code``, ``start_date``, and ``created``
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
      {
      "membership": {
        "tags": [],
        "status": 10,
        "status_text": "Blue",
        "stamps": 10,
        "balance": 20,
        "is_blocked": false,
        "referral_code": "asd",
        "start_date": "2020-01-01",
        "created": "2020-01-01",
      },
    }


When some fields don't validate:

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"errors": {"info": "Transaction can't be canceled due to insufficient Stamps"}}

C. Legacy Endpoint
------------------
Legacy endpoint's documentation is available at `Legacy transaction API <http://docs.stamps.co.id/en/latest/legacy_transaction_api.html>`_



4. Modify Transaction's Value or Items
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



5. Getting Transaction Detail
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


6. Preview Transaction Earnings
=============================
| URL endpoint: https://stamps.co.id/api/v2/transactions/preview-earnings
| Allowed method: GET
| Requires authentication: Yes


A. Request
-----------------------------

You can preview transaction's earning data before creating a transaction through this API.

==========================  =========== =========================================================
Parameter                   Required    Description
==========================  =========== =========================================================
token                       Yes         Authentication string
user                        No          Email address / Member ID indicating customer.
                                        Leaving this empty creates an ``open`` transaction.
store                       Yes         A number (id) indicating store where transaction
                                        is created
total_value                 Yes         A number indicating transaction's grand total
sub_total                   No          A number indicating transaction subtotal
discount                    No          A number indicating transaction discount (in Rp.)
service_charge              No          A number indicating service charge (in Rp.)
tax                         No          A number indicating transaction tax (in Rp.)
channel                     No          Channel of a transaction, for channel mapping, see table below
type                        No          The type of prepared transactions, for type mapping, see table below
items                       No          List of items containing product name, quantity, subtotal,
                                        stamps_subtotal (optional) & eligible_for_stamps.
                                        ``price`` is the combined price of products (qty * unit price),
                                        ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps),
                                        this field is optional.
                                        ``eligible_for_stamps`` is boolean value to determine whether the item should be included in Stamps Calculation. Defaults to ``true``.
payments                    No          List of payments object containing value, payment_method, and
                                        eligible_for_membership(optional).
                                        ``value`` is the amount of payment
                                        ``payment_method`` is the method used for payment
                                        ``eligible_for_membership`` whether this payment is used for member's status/level changes.
                                        This field is optional. Default to true if not provided(can be configured later).
==========================  =========== =========================================================


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/v2/transactions/preview-earnings -i -d '{ "token": "secret", "user": "customer@stamps.co.id", "store": 422, "tax":5000, "channel":1, "type":2, "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "subtotal": 10000}, {"product_name": "Iced Tea", "quantity": 4, "subtotal": 5000}]}, "payments": [{"value": 30000, "payment_method": 10}, {"value": 20000, "payment_method": 43, "eligible_for_membership": false}]'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
stamps              The amount of stamps to be received after completing the transaction.
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
      "stamps": 10
    }


1. List User Transaction
=============================
| URL endpoint: https://stamps.co.id/api/transactions/by-user
| Allowed method: GET
| Requires authentication: Yes


A. Request
-----------------------------

You can query latest user transaction's list through this API.

==========================  =========== =========================================================
Parameter                   Required    Description
==========================  =========== =========================================================
token                       Yes         Authentication string
user                        Yes         A string indicating customer's email, Member ID,
                                        mobile number or primary key ID
==========================  =========== =========================================================


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X GET -H "Content-Type: application/json" https://stamps.co.id/api/transactions/by-user -i -d '{ "token": "secret", "user": 123}'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
transactions        list of user transactions
                    contains, id, value,
                    stamps_earned, number_of_people,
                    discount, subtotal, invoice_number,
                    created, merchant, and store
=================== ==============================


C. Example Response
-------------------

On successful get Transactions:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

    {
    "transactions": [
      {
        "id": 20,
        "value": 200000.0,
        "stamps_earned": 1,
        "number_of_people": null,
        "discount": null,
        "subtotal": null,
        "invoice_number": "0020014795:1:001",
        "created": 1661075448,
        "merchant": "Merchant Test",
        "store": {
          "name": "0020014795",
          "display_name": "TEST STORE"
        }
      },
      {
        "id": 15,
        "value": 10000.0,
        "stamps_earned": 20,
        "number_of_people": null,
        "discount": null,
        "subtotal": 102.0,
        "invoice_number": "0020014795:1:002",
        "created": 1661075448,
        "merchant": "Merchant Test"",
        "store": {
          "name": "0020014795",
          "display_name": "TEST STORE"
        }
      }
    ]
  }
