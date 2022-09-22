************************************
Activity API
************************************

1. Get Activity List
====================
| URL endpoint: https://stamps.co.id/api/v2/activities/
| Allowed method: GET
| Requires authentication: Yes


A. Request
----------

You can query a user's activity list by calling the API with these parameters. Only 50 activities will be returned per API call.

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
user                Yes         Email/mobile number identifying the member
token               Yes         Merchant's Authentication token
older_than          No          Activity ID. 50 activities will be returned that are older than this ID. If not provided, will return the latest 50 activities.
=================== =========== =======================

Here's an example of a Activity List API call using cURL.

.. code-block :: bash
    
    $ curl 'https://stamps.co.id/api/v2/activities?token=abc&user=customer@stamps.co.id'

B. Response
-----------

On a successful API call, Stamps will reply with 50 (maximum) activities in a JSON List:

=================== ==================
Variable            Description
=================== ==================
actitivities        Activity list.
                    Contains id, number of people and other details.
error_messages      Errors encountered when parsing data (if any)
=================== ==================

C. Example Response
-------------------

Below is an example response on successful API call.

.. code-block:: javascript

    HTTP/1.0 200 OK
    Allow: GET, HEAD, OPTIONS
    Content-Type: application/json
    Date: Wed, 15 Dec 2020 07:02:10 GMT
    Server: WSGIServer/0.1 Python/2.7.4
    Vary: Accept, Cookie

    {
        "activities":[{
            "id":2590959,
            "type":1,
            "created":"2020-12-04T02:42:44+00:00",
            "created_timestamp":1607049764,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"Update Database Voucher IDR 100,000",
            "status":2,
            "store":"Tes Store",
            "store_display_name":""
        },
        {
            "id":2588920,
            "type":1,
            "created":"2020-12-02T08:40:00+00:00",
            "created_timestamp":1606898400,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"VOUCHER TEST REDEEM Rp 100,000",
            "status":1,
            "store":"Campaign Microsite",
            "store_display_name":"Campaign Microsite"
        },
        {
            "id":2588910,
            "type":1,
            "created":"2020-12-02T07:35:58+00:00",
            "created_timestamp":1606894558,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"VOUCHER TEST REDEEM Rp 100,000",
            "status":1,
            "store":"Campaign Microsite",
            "store_display_name":"Campaign Microsite"
        },
        {
            "id":2587493,
            "type":1,
            "created":"2020-12-01T03:40:40+00:00",
            "created_timestamp":1606794040,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"Update Database Voucher IDR 100,000",
            "status":2,
            "store":"Tes Store",
            "store_display_name":""
        },
        {
            "id":2587476,
            "type":1,
            "created":"2020-12-01T03:00:06+00:00",
            "created_timestamp":1606791606,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"Update Database Voucher IDR 100,000",
            "status":2,
            "store":"Tes Store",
            "store_display_name":""
        },
        {
            "id":2375302,
            "type":1,
            "created":"2020-06-18T05:26:08+00:00",
            "created_timestamp":1592457968,
            "merchantName":"Levi's",
            "merchantID":2,
            "stamps":0,
            "name":"salah"
            "status":1,
            "store":"levisecom",
            "store_display_name":"Levi's Ecommerce"
        },
        {
            "id": 704,
            "type": 0,
            "created": "2022-08-31T06:01:01+00:00",
            "created_timestamp": 1661925661,
            "merchantName": "Ace Hardware",
            "merchantID": 5,
            "stamps": 20,
            "name": "Transaction #493",
            "status": 2,
            "store": "A301",
            "store_display_name": "ST ACE KARAWACI MAL",
            "invoice_number": "INV-17",
            "channel": "Mobile App"
        }]
    }


Activity Type Mapping

=================== ===========
Code                Description
=================== ===========
0                   Transaction
1                   Redemption
2                   Awarded Stamps
5                   Membership Upgrade
6                   Membership Downgrade
7                   Change Balance
8                   Survey Submission
9                   Completed Registration
10                  Deduct Stamps
=================== ===========

