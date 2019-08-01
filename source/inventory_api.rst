************************************
Inventory API
************************************


`1. Add / Update Inventory`_

`2. Update Out of Stock Inventory`_


1. Add / Update Inventory
==================
| URL endpoint: https://host.com/api/backend/inventories/update
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can add or update a product variant in the store inventory by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         merchant token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token fc9a483c04a6b66745b26741aa24f3d0d29b9d84

Body
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
store_code          Yes         Store Code string
inventories         Yes         An array of product variant objects
=================== =========== =======================

Each product variant object in the array has the following parameters 

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
variant_code        Yes         variant code string
price               Yes         price number
out_of_stock        No          boolean, defaults to false
is_active           No          boolean, defaults to false
=================== =========== =======================

Here's an example of how the API call might look like in JSON format:

.. code-block::

    {
        "store_code": "Store1",
        "inventories": [
            {
                "variant_code": "burger-1",
                "price": 36364,
                "out_of_stock": false,
                "is_active": true
            },
            {
                [Redacted Content]
            },
            {
                [Redacted Content]
            },
            {
                [Redacted Content]
            },
            {
                [Redacted Content[
            }
        ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token fc9a483c04a6b66745b26741aa24f3d0d29b9d84" -H "Expect:" https://host.com/api/backend/inventories/update -i -d '{ "store_code": "Store1", "inventories": [{"variant_code": "burger-1", "price": 36364, "out_of_stock": false, "is_active": true}] }'


B. Response
-----------


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


If call to inventory API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    "status: 'ok'"

When some fields don't validate (JSON):

Empty or invalid store_code field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Store not found",
        "error_code": "store_not_found"
    }
    
Empty variant_code field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "This field is required.",
        "error_code": "required",
        "errors": {
            "variant_code": "This field is required."
        }
    }
    
Invalid variant_code field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Variant not found",
        "error_code": "variant_not_found",
        "errors": {
            "variant_code": "Variant not found"
        }
    }
    
Invalid code field (too long)

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Ensure this value has at most 10 characters (it has 13).",
        "error_code": "max_length",
        "errors": {
            "variant_code": "Ensure this value has at most 10 characters (it has 13)."
        }
    }
    
Empty price field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "This field is required.",
        "error_code": "required",
        "errors": {
            "price": "This field is required."
        }
    }
    
Invalid price field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Enter a number.",
        "error_code": "invalid",
        "errors": {
            "price": "Enter a number."
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


2. Update Out of Stock Inventory
================================
| URL endpoint: https://host.com/api/backend/inventories/update-out-of-stock
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can add or update a product variant in the store inventory by calling the API with these parameters

Header
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         merchant token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token fc9a483c04a6b66745b26741aa24f3d0d29b9d84

Body
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
store               Yes         Store Code string
code                Yes         variant code string
out_of_stock        No          boolean, defaults to false
=================== =========== =======================

Here's an example of how the API call might look like in JSON format:

.. code-block::

    {
        "store": "Store1",
        "code": "burger-1",
        "out_of_stock": true
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token fc9a483c04a6b66745b26741aa24f3d0d29b9d84" -H "Expect:" https://host.com/api/backend/inventories/update-out-of-stock -i -d '{ "store": "Store1", "code": "burger-1", "out_of_stock": true }'


B. Response
-----------


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


If call to inventory API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    "status: 'ok'"

When some fields don't validate (JSON):

Empty store field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "This field is required.",
        "error_code": "required",
        "errors": {
            "store": "This field is required."
        }
    }
    
Invalid store field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Select a valid choice. That choice is not one of the available choices.",
        "error_code": "invalid_choice",
        "errors": {
            "store": "Select a valid choice. That choice is not one of the available choices."
        }
    }
    
Empty code field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "This field is required.",
        "error_code": "required",
        "errors": {
            "code": "This field is required."
        }
    }
    
Invalid code field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Test1 is not a valid variant in store Store1",
        "error_code": "invalid_code",
        "errors": {
            "__all__": "Test1 is not a valid variant in store Store1"
        }
    }
    
Invalid code field (too long)

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Ensure this value has at most 10 characters (it has 13).",
        "error_code": "max_length",
        "errors": {
            "code": "Ensure this value has at most 10 characters (it has 13)."
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
