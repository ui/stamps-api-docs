************************************
Content Type
************************************

**All POST** requests must be JSON encoded, having “application/json” as Content-Type. Here’s an example on how to specify an extra content type header in request using cURL

.. code-block :: bash

    > $ curl –X POST –H “Content-Type: application/json”
