************************************
Token API
************************************

1. Get a Token
=======================
| URL endpoint: https://stamps.co.id/api/token/
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
    https://stamps.co.id/api/token/

B. Response
-----------
On a successful API call, Stamps will give you the accss_token that can be used to access our other APIs.

=================== ==================
Variable            Description
=================== ==================
access_token        Token that can be used to access Stamps APIs.
=================== ==================

access_token itself is encoded with base64 and signed. Here is an example of an access_token:

.. code-block:: bash

    eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MjY2NTcwLCJpYXQiOjE2NTcxODAxNzAsImp0aSI6IjE1NjExODk1ZGQ1MDQxNzFhZDMwY2M2ZTBjYzY1YTNhIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfdXNlcl9pZCI6NTg3MH0.ay-2IQ1hkF6kI51e9eFOXzBBFDR30cD2nluhnShjzNg

Each part is separated by a '.'

.. code-block:: bash

    eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
| is the header which have the information of the algorithm in use.
.. code-block:: bash

    eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MjY2NTcwLCJpYXQiOjE2NTcxODAxNzAsImp0aSI6IjE1NjExODk1ZGQ1MDQxNzFhZDMwY2M2ZTBjYzY1YTNhIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfdXNlcl9pZCI6NTg3MH0
| is the actual payload where store information that is needed for client. Below is the decoded version of the payload:

.. code-block:: javascript

    {
        "token_type":"access",
        "exp":1657266570,"iat":1657180170,
        "jti":"15611895dd504171ad30cc6e0cc65a3a",
        "user_id":5870,
        "merchant_user_id":5870
    }

.. code-block:: bash
    
    ay-2IQ1hkF6kI51e9eFOXzBBFDR30cD2nluhnShjzNg
| is the signature.
|
| You can then use this token as your authorization header as ``Authorization: Bearer <JWT Token>``

2. Verify a Token
=======================
| URL endpoint: https://stamps.co.id/api/token/verify/
| Allowed method: POST
| Requires authentication: Yes




A. Request
-----------------------------

You can get verify a token by calling the API with the these data


=========================== =========== =======================
Parameter                   Required    Description
=========================== =========== =======================
access_token                Yes         Authentication string
=========================== =========== =======================


Here's an example of an API call using cURL.

.. code-block:: bash
    
    $ curl \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"accesstoken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU3MjY2NTcwLCJpYXQiOjE2NTcxODAxNzAsImp0aSI6IjE1NjExODk1ZGQ1MDQxNzFhZDMwY2M2ZTBjYzY1YTNhIiwidXNlcl9pZCI6NTg3MCwibWVyY2hhbnRfdXNlcl9pZCI6NTg3MH0.ay-2IQ1hkF6kI51e9eFOXzBBFDR30cD2nluhnShjzNg", "merchant": 1}' \
    https://stamps.co.id/api/token/verify/


B. Response
-----------
On a successful API call, Stamps will give you status code ``200`` with empty payload.
