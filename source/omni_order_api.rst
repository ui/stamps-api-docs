************************************
Omni Order API
************************************


`1. Sync Order`_

`2. Get Order`_

`3. Get Order Details`_

`4. Confirm Order`_

`5. Mark Order as Paid`_

`6. Complete Order`_

`7. Cancel Order`_

`B. Generic Response`_ - Common across all API calls

`C. Specific Responses`_ - Lists down specific behaviors and responses of each API call

The various API calls contain similar headers and body. The differences will be highlighted within each API call


1. Sync Order
====================
| URL endpoint: https://host.com/api/store/orders/sync
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

You get all new orders which have not been synced before by calling the API with these parameters

It will only return said orders only **once**.

The store is derived from the store token string.

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

Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://host.com/api/store/orders/sync
    

B. Response
-----------

In response to these API calls, Omni will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
orders              An array of order objects
=================== ==================

**ALL** new orders will be synced, but Omni only replies with an array of up to 100 order objects which have not been synced, and each order object has the following `fields`_.

The order objects will only be returned **once**.

If all orders are synced, it returns an empty array :code:`{ "orders" : [] }`


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
                [Redacted Content]
            }
        ]
    }

If missing or wrong authentication token:

.. code-block:: bash

    HTTP/1.0 401 UNAUTHORIZED
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]
    
    {"detail": "Invalid token"}

If HTTP is used instead of HTTPS:

.. code-block:: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]

    {"detail": "Please use https instead of http"}

    
2. Get Order
====================
| URL endpoint: https://host.com/api/store/orders/get
| Allowed method: GET
| Requires authentication: Yes

A. Request
----------

You can retrieve the latest 15 orders by calling the API with these parameters

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

    $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://host.com/api/store/orders/get?last_order_id=0
    

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
                [Redacted Content]
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

If missing or wrong authentication token:

.. code-block:: bash

    HTTP/1.0 401 UNAUTHORIZED
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]
    
    {"detail": "Invalid token"}

If HTTP is used instead of HTTPS:

.. code-block:: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]

    {"detail": "Please use https instead of http"}


3. Get Order Details
====================
| URL endpoint: https://host.com/api/store/orders/details
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

    $ curl -X GET -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://host.com/api/store/orders/details?number=FR9TL74P
    

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

If missing or wrong authentication token:

.. code-block:: bash

    HTTP/1.0 401 UNAUTHORIZED
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]
    
    {"detail": "Invalid token"}

If HTTP is used instead of HTTPS:

.. code-block:: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: GET, OPTIONS
    [Redacted Header]

    {"detail": "Please use https instead of http"}


4. Confirm Order
==================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/confirm
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Confirmed by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

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

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/confirm -i -d '{ "number": "FR9TL74P" }'
    

Response
----------

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

An example of the response in JSON is found `here`_.

Confirm Order changes the :code:`"status"` field from 1 (new) to 10 (confirmed) and the :code:`"statusText"` from "New" to "Confirmed".

If an order is already confirmed, complete, or cancelled, the API call will return an error response stating that.


5. Mark Order as Paid
==================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/paid
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Paid by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

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

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/paid -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

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

An example of the response in JSON is found `here`_.

Mark Order as Paid changes the :code:`"paymentStatus"` field from 1 (unpaid) to 2 (paid).

If an order is already paid or cancelled, the API call will return an error response stating that.


6. Complete Order
==================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/complete
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can mark an order as Complete by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

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

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/complete -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

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

An example of the response in JSON is found `here`_.

Complete Order changes the :code:`"paymentStatus"` field to 2 (paid), :code:`"status"` field to 20 (complete) and the :code:`"statusText"` field to "Complete" regardless of the values within the fields beforehand except for the condition(s) below.

If an order is already complete or cancelled, the API call will return an error response stating that.


7. Cancel Order
==================
| URL endpoint: https://orders.upnormal.co.id/api/store/orders/cancel
| Allowed method: POST
| Requires authentication: Yes

A. Request
------------

You can mark an order as Confirmed by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF

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

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token vE53k50FVtct50ll8iHBE6FgMRVCyJeF" -H "Expect:" https://orders.upnormal.co.id/api/store/orders/cancel -i -d '{ "order": "FR9TL74P" }'
    
Response
----------

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

An example of the response in JSON is found `here`_.

Cancel Order changes the :code:`"status"` field to 30 (cancelled) and the :code:`"statusText"` field to "Cancelled". This action will cause the order to be inaccessible to the other 3 API calls and **cannot be reversed**.

If an order is already cancelled, the API call will return an error response stating that.

.. _here:

9. Response Example
=====================

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
        "status": 10,
        "deliveryStatus": 10,
        "statusText": "Confirmed",
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
            "number": "Order not found"
        }
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

Examples of failed API call responses
_______________________________________

These are various examples of the error responses returned by failed API calls described above:

Common Header

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]
    
Body (Confirmed Order, Paid Order, Complete Order, Cancelled Order errors)

.. code-block:: json

    {
        "error_message": "Order has been confirmed",
        "error_code": "invalid_status",
        "errors": {
            "number": "Order has been confirmed"
        }
    }

    {
        "error_message": "Order already paid",
        "error_code": "already_paid",
        "errors": {
            "order": "Order already paid"
        }
    }

    {
        "error_message": "Order already completed",
        "error_code": "order_already_completed",
        "errors": {
            "order": "Order already completed"
        }
    }

    {
        "error_message": "Order already canceled",
        "error_code": "order_already_canceled",
        "errors": {
            "order": "Order already canceled"
        }
    }
