*********************************************
Temporary Access Token Authentication (DRAFT)
*********************************************

1. Get a Token
=======================
| URL endpoint: https://stamps.co.id/api/auth/get-access-token/
| Allowed method: POST
| Requires authentication: 


A. Request
-----------------------------

You can get a new token by calling the API with the these data


=========================== =========== =======================
Parameter                   Required    Description
=========================== =========== =======================
token                       Yes         Authentication string
merchant                    Yes         Merchant User ID indicated which merchant will the user access
=========================== =========== =======================


Here's an example of an API call using cURL.

.. code-block:: bash
    
    $ curl \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"token": "23095sgtr95402mkdls954002", "merchant": 1}' \
    https://stamps.co.id/api/auth/get-access-token/

B. Response
-----------

Stamps will give you a time limited JWT token that can be used to access our other APIs.

=================== ==================
Variable            Description
=================== ==================
access_token        Token that can be used to access Stamps APIs.
=================== ==================

Example of access token is below:

.. code-block:: bash
    
    eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MzMyNjE2LCJpYXQiOjE2NTcyNDYyMTYsImp0aSI6IjRlYWRjNDAxNGQwZDRkNzc4NjkxYjg0ZDU3MGE2ZGFmIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfaWQiOjU4NzB9.6i9vKIuKdRUHUCv6UJ8VBBWzr1UPedVQarAr3rjtuJY
| You can use `jwt.io <https://jwt.io>`_ to debug  the above token with secret ``TESTJWT``
| You can then use this token as your authorization header as:

.. code-block:: bash

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MzMyNjE2LCJpYXQiOjE2NTcyNDYyMTYsImp0aSI6IjRlYWRjNDAxNGQwZDRkNzc4NjkxYjg0ZDU3MGE2ZGFmIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfaWQiOjU4NzB9.6i9vKIuKdRUHUCv6UJ8VBBWzr1UPedVQarAr3rjtuJY

2. Access Token Verification
=============================
| URL endpoint: https://stamps.co.id/api/auth/verify-token/
| Allowed method: POST
| Requires authentication: Yes
|
| You can verify whether your access token is valid via this API end point.

A. Request
-----------------------------

You can verify a token by calling the API with this header


=========================== =========== =======================
Parameter                   Required    Description
=========================== =========== =======================
Authorization               Yes         JWT Bearer token
=========================== =========== =======================


Here's an example of an API call using cURL.

.. code-block:: bash
    
    $ curl \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MzMyNjE2LCJpYXQiOjE2NTcyNDYyMTYsImp0aSI6IjRlYWRjNDAxNGQwZDRkNzc4NjkxYjg0ZDU3MGE2ZGFmIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfaWQiOjU4NzB9.6i9vKIuKdRUHUCv6UJ8VBBWzr1UPedVQarAr3rjtuJY" \
    https://stamps.co.id/api/auth/verify-token/



B. Response
-----------
This will return the payload of JWT Token:

.. code-block:: javascript

    {
        "token_type": "access",
        "exp": 1657332616,
        "iat": 1657246216,
        "jti": "4eadc4014d0d4d778691b84d570a6daf",
        "user_id": 5870,
        "merchant_id": 5870
    }
