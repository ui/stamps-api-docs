************************************
Sync Order API
************************************


`1. Get Order Details`_

`2. Get Order`_

`3. Sync Order`_


1. Get Order Details
====================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/details
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

You can get a specific order's details by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         store token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

Query Parameter
_______________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
number              Yes         Order number string
=================== =========== =======================


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/details?number=FR9TL74P
    

B. Response
-----------

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
order               Order object
=================== ==================

.. _parameters:
Omni replies with an order object that contains the following data:

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
items               An array of item objects
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
        "order": {
            "id": 1,
            "number": "FR9TL74P",
            "total": 36364.0,
            "tax": 3636.0,
            "serviceCharge": 0.0,
            "grandTotal": 41000.0,
            "tableNumber": "A1",
            "channel": 2,
            "notes": "No lettuce",
            "status": 1,
            "deliveryStatus": 10,
            "statusText": "New",
            "storeName": "store test",
            "userID": 1,
            "created": 1564045835,
            "paymentMethod": 1,
            "paymentStatus": 1,
            "user": {
                "id": 1,
                "name": "test",
                "phone": "+628111111111"
            },
            "items": [
                {
                    "id": 1,
                    "notes": "",
                    "subtotal": 36364.0,
                    "quantity": 1,
                    "variant": {
                        "id": 1,
                        "code": "BURGER01",
                        "sku": "BURGER01",
                        "name": "Burger",
                        "displayName": "",
                        "isActive": true,
                        "upsizedVersion": null
                    },
                    "modifiers": []
                }
            ],
            "delivery_info": null
        }
    }

When some fields don't validate (JSON):

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Your request cannot be completed",
        "error_code": "invalid_request"
    }

If missing or wrong authentication token:

.. code-block:: bash

    HTTP/1.0 401 UNAUTHORIZED
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
    {"detail": "Invalid token"}

If HTTP is used instead of HTTPS:

.. code-block:: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Please use https instead of http"}
    
    
2. Get Order
====================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/get
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

You can get a specific order's details by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         store token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

Query Parameter
_______________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
last_order_id       Yes         last order id
=================== =========== =======================


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/details?last_order_id=0
    

B. Response
-----------

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
orders              An array of order objects
=================== ==================

Omni replies with an array of up to 25 order objects wherein each order object has the following `parameters <parameters>`_. (Click the parameters link to view the order object parameters)
