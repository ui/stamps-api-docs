************************************
Transaction API
************************************

1. Adding new transaction
=============================
| URL endpoint: https://stamps.co.id/api/transactions/add
| Allowed Method: POST
| Require Authentication: Yes
| Expected Content Type: application/json, application/xml
| Response Content Type: application/json(default), application/xml


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


Here's an example of how the API call might look like in JSON format:

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
                 "price": 10000
              },
              {
                 "product_name": "Iced Tea",
                 "quantity": 4,
                 "price": 5000
              }
           ]
        }


Example of API call request using cURL (JSON)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/transactions/add -i -d '{ "token": "aaabbbcccdddeeeffff", "user_email": "Customer@stamps.co.id", "store": 2, "invoice_number": "abc123", "total_value": 50000, "subtotal": 40000, "discount": 0, "service_charge": 5000, "tax": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "price": 10000}, {"product_name": "Iced Tea", "quantity": 4, "price": 5000}]}'

Example of API call request using cURL (XML)

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/xml" -H "Accept: application/xml" https://stamps.co.id/api/transactions/add -i -d '<?xml version="1.0" encoding="UTF-8" ?>
    <root>
        <token>aaaabbbbccccddddeeeefffff</token>
        <user_email>customer@stamps.co.id</user_email>
        <store>84</store>
        <total_value>50000</total_value>
        <subtotal>40000</subtotal>
        <discount>0</discount>
        <service_charge>5000</service_charge>
        <tax>5000</tax>
        <items>
            <list-item>
                <product_name>Cappucino</product_name>
                <quantity>2</quantity>
                <price>10000</price>
            </list-item>
            <list-item>
                <product_name>Iced Tea</product_name>
                <quantity>4</quantity>
                <price>5000</price>
            </list-item>
        </items>
    </root>'

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

Response content type can be set using the `Accept` header made in the request :

.. code-block :: bash

  $ curl -X POST -H "Content-Type: application/xml" -H "Accept: application/xml" # Response will be in XML
  $ curl -X POST -H "Content-Type: application/xml" # Response will be in JSON(default)

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
    405                 HTTP method not allowed - the
                        requested resources cannot be called with the specified HTTP method
    500, 502, 503, 504  Server Errors - something is
                        wrong on Stamps' end
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

If transaction is successful(XML):

.. code-block :: bash

      HTTP/1.0 200 OK
      Vary: Accept
      Content-Type: application/xml
      Allow: POST, OPTIONS
       [Redacted Header]

      <?xml version="1.0" encoding="utf-8"?>
      <root>
        <customer>
          <status>Blue</status>
          <id>16</id>
          <stamps_remaining>5</stamps_remaining>
        </customer>
        <transaction>
          <stamps_earned>5.0</stamps_earned>
          <id>1</id>
          <value>50000</value>
        </transaction>
      </root>



When some fields don't validate (JSON):

.. code-block :: bash

      HTTP/1.0 400 BAD REQUEST
      Vary: Accept
      Content-Type: application/json
      Allow: POST, OPTIONS
       [Redacted Header]


      {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"price": "This field is required."}, {"invoice_number": "Store does not exist"}]}


When some fields don't validate(XML):

.. code-block :: bash

      HTTP/1.0 400 BAD REQUEST
      Vary: Accept
      Content-Type: application/json
      Allow: POST, OPTIONS
       [Redacted Header]

      <?xml version="1.0" encoding="utf-8"?>
      <root>
        <validation_errors>
          <list-item>
            <price>This field is required.</price>
          </list-item>
          <list-item>
            <store>Select a valid choice. That choice is not one of the available choices.</store>
          </list-item>
        </validation_errors>
        <detail>
          Your transaction cannot be completed due to the following error(s)
        </detail>
      </root>


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
