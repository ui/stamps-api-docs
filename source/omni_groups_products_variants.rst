************************************
Groups, Products, Variants API
************************************

`1. Add / Update Group`_

`2. Add / Update Variant Group`_

`3. Add / Update Product & Variant`_


1. Add / Update Group
=======================
| URL endpoint: https://host.com/api/backend/groups/update
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can add or update multiple groups by calling the API with these parameters:

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         merchant token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-merchant-token

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
                "name": "SPECIALS",
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

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-merchant-token" -H "Expect:" https://host.com/api/backend/groups/update -i -d '{ "groups": [{ "code": "10", "name": "SPECIAL DRINK", "parent": "1" }] }'


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
    
Empty name field

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
            "name": "This field is required."
        }
    }
    
Empty or invalid parent field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Parent group not found",
        "error_code": "parent_not_found",
        "errors": {
            "parent": "Parent group not found"
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
    

2. Add / Update Variant Group
===============================
| URL endpoint: https://host.com/api/backend/groups/set-variants
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can add or update variants into a group by calling the API with these parameters:

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         merchant token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-merchant-token

Body
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
code                Yes         Group code
variants            Yes         Array of variant codes
=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
        "groups": [  
            {
                "code": "10",
                "variants": ["burger-1", "burger-2"]
            }
        ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-merchant-token" -H "Expect:" https://host.com/api/backend/groups/set-variants -i -d '{ "groups": [{ "code": "10", "variants": ["burger-1", "burger-2"] }] }'


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
    
Empty variants field

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
            "variants": "This field is required."
        }
    }
    
Invalid variant code in variants field

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
        "error_message": "Select a valid choice. notvalidcode is not one of the available choices.",
        "error_code": "invalid_choice",
        "errors": {
            "variants": "Select a valid choice. notvalidcode is not one of the available choices."
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
    

3. Add / Update Product & Variant
===================================
| URL endpoint: https://host.com/api/backend/products/update
| Allowed method: POST
| Requires authentication: Yes

A. Request
----------

You can add or update multiple groups by calling the API with these parameters:

HTTP Header
___________

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
Content-Type        Yes         application/json
Authorization       Yes         merchant token string
=================== =========== =======================

.. code-block::

    Content-Type: application/json
    Authorization: token example-merchant-token

Body
______

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================

=================== =========== =======================


Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
    
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block:: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Authorization: token example-merchant-token" -H "Expect:" https://host.com/api/backend/products/update -i -d '{  }'


B. Response
-----------

=================== ==================
Variable            Description
=================== ==================

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


If call to inventory API is successful (JSON):

.. code-block:: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    

When some fields don't validate (JSON):

.. code-block:: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]


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
