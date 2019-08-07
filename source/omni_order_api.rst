************************************
Omni Order API
************************************


`1. Sync Order`_

`2. Get Order Details`_

`3. Confirm Order`_

`4. Mark Order as Paid`_

`5. Complete Order`_

`6. Cancel Order`_

`7. Status Code Mappings`_

`8. Error Responses`_


1. Sync Order
====================
| URL endpoint: https://host.com/api/store/orders/sync
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

This API endpoint should used by POS terminals to fetch incoming orders from OMNI. This API is lightweight and suitable to be used for polling for new orders.

This endpoint will only return said orders exactly **once**.

The store is derived from the store token string.

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         store token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-store-token

Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/sync
    

B. Response
-----------

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
orders              An array of order objects
=================== ==================

Up to 100 new orders will be returned, each order object has the following `fields`_.

The order objects will only be returned **once**.

If there are no new orders, it returns an empty array :code:`{ "orders" : [] }`


Here are examples of API responses:


If call to sync order API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]

    {
        "orders": [
            {
                [Redacted Content]
            },
            {
                [Redacted Content]
            },
            {
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
                "items": [],
                "delivery_info": null
            }
        ]
    }

    
..  Get Order
    ====================
    | URL endpoint: https://host.com/api/store/orders/get
    | Allowed method: GET
    | Requires authentication: Yes

    A. Request
    ----------

    You can retrieve the latest 15 orders by calling the API with these parameters:

    HTTP Header
    ___________

    =================== =========== =======================
    Parameter           Required    Description
    =================== =========== =======================
    Content-Type        Yes         application/json
    Authorization       Yes         store token string
    =================== =========== =======================

    .. code-block::

        Content-Type: application/json
        Authorization: token example-store-token

    Query Parameter
    _______________

    =================== =========== =======================
    Parameter           Required    Description
    =================== =========== =======================
    last_order_id       Yes         last order id
    =================== =========== =======================


    Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

    .. code-block:: bash

        $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/get?last_order_id=0
        

    B. Response
    -----------

    In response to these API calls, Omni will reply with the following data in JSON:

    =================== ==================
    Variable            Description
    =================== ==================
    orders              An array of order objects
    =================== ==================

    Omni replies with an array of the latest 15 order objects wherein each order object has the following `fields`_.


    Here are examples of API responses:


    If call to sync order API is successful (JSON):

    .. code-block:: bash

        HTTP/1.0 200 OK
        Vary: Accept
        Content-Type: application/json
        Allow: GET, OPTIONS
        [Redacted Header]

        {
            "orders": [
                {
                    [Redacted Content]
                },
                {
                    [Redacted Content]
                },
                {
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
                    "items": [],
                    "delivery_info": null
                }
            ]
        }

    When some fields don't validate (JSON):

    .. code-block:: bash

        HTTP/1.0 400 BAD REQUEST
        Vary: Accept
        Content-Type: application/json
        Allow: GET, OPTIONS
        [Redacted Header]

        {
            "error_message": "Invalid last order id",
            "error_code": "invalid_last_order_id",
            "errors": {
                "last_order_id": "Invalid last order id"
            }
        }


2. Get Order Details
====================
| URL endpoint: https://host.com/api/store/orders/details
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

You can get a specific order's details by calling the API with these parameters:

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         store token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-store-token

Query Parameter
_______________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
number              Yes         Order number string
=================== =========== =======================


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/details?number=FR9TL74P
    

B. Response
-----------

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
order               Order object
=================== ==================

.. _fields:

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


If call to sync order API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
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
            "storeName": "BURGER GOD",
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
    Allow: GET, OPTIONS
    [Redacted Header]

    {
        "error_message": "Your request cannot be completed",
        "error_code": "invalid_request"
    }


3. Confirm Order
==================
| URL endpoint: https://host.com/api/store/orders/confirm
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Confirmed by calling the API with these parameters:

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
number              Yes         Order number string
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
      "number": "FR9TL74P"
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/confirm -i -d '{ "number": "FR9TL74P" }'
    

Response
----------

Confirm Order changes the :code:`"status"` field from 1 (new) to 10 (confirmed) and the :code:`"statusText"` from "New" to "Confirmed".

If an order is already confirmed, complete, or cancelled, the API call will return an error response stating that.

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
        "status": 30,
        "deliveryStatus": 10,
        "statusText": "Cancelled",
        "storeName": "Burger God",
        "userID": 1,
        "created": 1564045835,
        "paymentMethod": 1,
        "paymentStatus": 1,
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
    
Order already confirmed

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
    {
        "error_message": "Order has been confirmed",
        "error_code": "invalid_status",
        "errors": {
            "number": "Order has been confirmed"
        }
    }


4. Mark Order as Paid
==================
| URL endpoint: https://host.com/api/store/orders/paid
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Paid by calling the API with these parameters:

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
order               Yes         Order number string
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
      "order": "FR9TL74P"
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/paid -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

Mark Order as Paid changes the :code:`"paymentStatus"` field from 1 (unpaid) to 2 (paid).

If an order is already paid or cancelled, the API call will return an error response stating that.

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
        "status": 30,
        "deliveryStatus": 10,
        "statusText": "Cancelled",
        "storeName": "Burger God",
        "userID": 1,
        "created": 1564045835,
        "paymentMethod": 1,
        "paymentStatus": 1,
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
    
Order already paid

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
    {
        "error_message": "Order already paid",
        "error_code": "already_paid",
        "errors": {
            "order": "Order already paid"
        }
    }


5. Complete Order
==================
| URL endpoint: https://host.com/api/store/orders/complete
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
order               Yes         Order number string
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
      "order": "FR9TL74P"
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


6. Cancel Order
==================
| URL endpoint: https://host.com/api/store/orders/cancel
| Allowed method: POST
| Requires authentication: Yes

A. Request
------------

You can mark an order as Confirmed by calling the API with these parameters:

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
order               Yes         Order number string
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
      "order": "FR9TL74P"
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-store-token" -H "Expect:" https://host.com/api/store/orders/cancel -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

Cancel Order changes the :code:`"status"` field to 30 (cancelled) and the :code:`"statusText"` field to "Cancelled". This action will cause the order to be inaccessible to the other 3 API calls and **cannot be reversed**.

If an order is already cancelled, the API call will return an error response stating that.

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
        "status": 30,
        "deliveryStatus": 10,
        "statusText": "Cancelled",
        "storeName": "Burger God",
        "userID": 1,
        "created": 1564045835,
        "paymentMethod": 1,
        "paymentStatus": 1,
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
    
Order already cancelled

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
    {
        "error_message": "Order already canceled",
        "error_code": "order_already_canceled",
        "errors": {
            "order": "Order already canceled"
        }
    }


7. Status Code Mappings
=========================


:code:`channel`

=========== ==============
Code        Definition
=========== ==============
1           Mobile App
2           POS
3           Kiosk
4           Web
=========== ==============


:code:`deliveryStatus`

=========== ==============
Code        Definition
=========== ==============
10          Dispatched
20          Completed
30          Confirmed
40          Cancelled
=========== ==============


:code:`orderStatus`

=========== ==============
Code        Definition
=========== ==============
1           New
10          Confirmed
20          Completed
30          Cancelled
40          Pending Payment
=========== ==============


:code:`paymentStatus`

=========== ==============
Code        Definition
=========== ==============
1           Unpaid
2           Paid
=========== ==============


:code:`paymentMethod`

=========== ==============
Code        Definition
=========== ==============
1           Cash
2           GO-PAY
3           Credit Card
4           Debit
5           Dana
6           OVO
=========== ==============


8. Error Responses
====================

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
