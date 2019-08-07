************************************
Groups, Products, Variants API
************************************

`1. Add / Update Group`_

`2. Add / Update Variant Group`_

`3. Add / Update Product & Variant`_


1. Add / Update Group
=======================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/complete
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Complete by calling the API with these parameters:

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-store-token

Body
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
code                Yes         Group code
name                Yes         Group name
parent              No          Parent group code
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
        "groups": [
            {
                "code": "10",
                "name": "SPECIAL DRINK",
                "parent": "1"
            },
            {
            [Redacted Content]
            },
            {
            [Redacted Content]
            },
            {
            [Redacted Content]
            }
        ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/complete -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

Complete Order changes the :code:`"paymentStatus"` field to 2 (paid), :code:`"status"` field to 20 (complete) and the :code:`"statusText"` field to "Complete" regardless of the values within the fields beforehand except for the condition(s) below.

If an order is already complete or cancelled, the API call will return an error response stating that.

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
id                  Order ID
number              Order number string
total               Total value before tax, service charge, delivery fee, discount and promotion
tax                 Tax value
serviceCharge       Service charge
grandTotal          Total value after tax, service charge, delivery fee, discount and promotion
tableNumber         Table number string
channel             An int representing where order was created (Mobile App, POS, Kiosk, Web)
notes               Customer notes string (example: no lettuce)
status              An int representing the order status ID
deliveryStatus      An int representing the delivery status ID
statusText          Order status string
storeName           Store name string
userID              ID of user making the order
created             Time when order is created in UNIX time
paymentMethod       An int representing the payment method ID
paymentStatus       An int representing the payment status ID
user                Contains the user's id, name and phone number
delivery_info       null
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
500, 502, 503, 504  Something went wrong on Omni's end
=================== ==============================


Here are examples of API responses:


If call to order status API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "id": 1,
        "number": "FR9TL74P",
        "total": 36364.0,
        "tax": 3636.0,
        "serviceCharge": 0.0,
        "grandTotal": 40000.0,
        "tableNumber": "A1",
        "channel": 2,
        "notes": "No lettuce",
        "status": 20,
        "deliveryStatus": 10,
        "statusText": "Completed",
        "storeName": "Burger God",
        "userID": 1,
        "created": 1564045835,
        "paymentMethod": 1,
        "paymentStatus": 2,
        "user": {
            "id": 1,
            "name": "user",
            "phone": "+628111111111"
        },
        "delivery_info": null
    }

When some fields don't validate (JSON):

Order not found

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Order not found",
        "error_code": "invalid_order_number",
        "errors": {
            "order": "Order not found"
        }
    }
    
Order already completed

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
    {
        "error_message": "Order already completed",
        "error_code": "order_already_completed",
        "errors": {
            "order": "Order already completed"
        }
    }
