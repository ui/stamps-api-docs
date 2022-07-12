************************************
Reward API
************************************

1. Querying for Available Rewards
=======================================
| URL endpoint: https://stamps.co.id/api/rewards/
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for all available rewards on stamps with optional checking to user's capability to redeem the rewards.

============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
token                          Yes         Authentication string
user                           No          A string indicating customer's email or Member ID
merchant                       Yes         Integer indicating merchant ID to be queried for reward
store                          Yes         Integer indicating store ID to be queried for reward
image_size                     No          Reward image size. Defaults to 200px x 200px
landscape_image_size           No          Reward image landscape size. Defaults to 545px x 300px 
only_redeemable_in_this_store  No          `true` or `false`. Defaults to `false`.
                                           If `true`, only rewards redeemable in given store will be returned.
include_inactive_vouchers      No          Boolean indicating response include inactive vouchers or not                                             
============================== =========== ===================================================================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/rewards/?token=abc&user=customer@stamps.co.id&merchant=14&store=1'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Customer information after successful query. Contains id, stamps_remaining, and status.
rewards             List of rewards available for redemption.
                    Contains id, name, stamps_required, extra_data, image_url, is_visible,
                    and redeemable(If user is provided)
vouchers            List of rewards available for redemption by user.
                    Contains  id, name, type, quantity, image_url, extra_data
                    landscape_url, and expires_on.
detail              Description of error (if any)
errors              Errors encountered when parsing
                    data (if any)
=================== ==============================


C. Response Headers
-------------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized – Often missing or
                    wrong authentication token
403                 Forbidden – You do not have
                    permission for this request
405                 HTTP method not allowed - The
                    requested resources cannot be called with the specified HTTP method
500, 502, 503, 504  Server Errors - something is
                    wrong on Stamps' end
=================== ==============================


D. Examples
-----------

On a successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "rewards": [
        {
          "stamps_to_redeem": 20,
          "image_url": "http://foo.com",
          "landscape_url": null,
          "id": 6,
          "membership": "Blue",
          "name": "Mee Goreng",
          "description": "reward description",
          "price": null,
          "extra_data": {
             "SKU": "A001SKU"
          },
          "merchant_code": null,
          "redeemable": true,
          "is_visible": true,
          "terms": "",
          "code": "A001",
          "type": 3
        },
        {
          "stamps_to_redeem": 60,
          "image_url": "http://foo.com",
          "landscape_url": null,
          "id": 5,
          "membership": "Blue",
          "name": "Curry Chicken",
          "description": "reward description",
          "price": null,
          "extra_data": {},
          "merchant_code": null,
          "redeemable": true,
          "is_visible": true,
          "terms": "",
          "code": "A002",
          "type": 3
        },
        {
          "stamps_to_redeem": 120,
          "image_url": "http://foo.com",
          "landscape_url": null,
          "id": 8,
          "membership": "Silver",
          "name": "Nasi Lemak",
          "description": "reward description",
          "price": null,
          "extra_data": {},
          "merchant_code": null,
          "redeemable": false,
          "is_visible": true,
          "terms": "",
          "code": "A003",
          "type": 3
        },
        {
          "stamps_to_redeem": 10,
          "image_url": "http://foo.com",
          "landscape_url": null,
          "id": 7,
          "membership": "Gold",
          "name": "Nasi Lemak",
          "description": "reward description",
          "price": null,
          "extra_data": {},
          "merchant_code": null,
          "redeemable": false,
          "is_visible": true,
          "terms": "",
          "code": "A004",
          "type": 3
        }
      ],
      "vouchers": [
        {
          "id": 9,
          "name": "Birthday Voucher",
          "code": "BD0201",
          "landscape_url": "foo-landscape.png",
          "image_url": "foo.png",
          "type": "promotion 1",
          "id": 110827,
          "expires_on": "13-02-2013 00:00",
          "terms": "input your birthday for get voucher on your birthday",
          "quantity": 1,
          "extra_data": {
             "SKU": "PROMO-birthday-20-off"
          }
        },
        {
          "id": 10,
          "name": "10 Year celebration promo",
          "code": "P010",
          "landscape_url": "foo-landscape.png",
          "image_url": "foo.png",
          "type": "promotion 1",
          "id": 110214,
          "expires_on": "24-01-2014 00:00",
          "terms": "sign up at stamps and get Free product A",
          "quantity": 2,
          "extra_data": {}
        }
      ],
      "customer": {
          "id": 114807,
          "stamps": 18,
          "membership_status": "Blue"
      }
    }


API call with missing parameters:


.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"reward": "This field is required"}]}


If missing or wrong authentication token:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {"detail": "Authentication credentials were not provided."}



2. Available Rewards By Merchant Group
=======================================
| URL endpoint: https://stamps.co.id/api/rewards/by-merchant-group
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for a merchant group's available rewards on stamps with optional checking to user's capability to redeem the rewards.

============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
token                          Yes         Authentication string
user                           No          A string indicating customer's email or Member ID
image_size                     No          Reward image size. Defaults to 200px x 200px
landscape_image_size           No          Reward image landscape size. Defaults to 545px x 300px 
show_all_reward                No          Boolean. Defaults to `false`.
                                           Only rewards applicable to user's membership level will be returned if set to `false`.
============================== =========== ===================================================================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/rewards/by-merchant-group?token=abc&user=customer@stamps.co.id&show_all_reward=true'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Customer information after successful query. Contains id, stamps_remaining, and status.
rewards             List of rewards available for redemption.
vouchers            List of vouchers available for redemption by user.
detail              Description of error (if any)
errors              Errors encountered when parsing
                    data (if any)
=================== ==============================


C. Response Headers
-------------------

=================== ==============================
Code                Description
=================== ==============================
200                 Everything worked as expected
400                 Bad Request - Often missing a
                    required parameter
401                 Unauthorized – Often missing or
                    wrong authentication token
403                 Forbidden – You do not have
                    permission for this request
405                 HTTP method not allowed - The
                    requested resources cannot be called with the specified HTTP method
500, 502, 503, 504  Server Errors - something is
                    wrong on Stamps' end
=================== ==============================



D. Examples
-----------

On a successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "rewards": [
        {
          "id": 1,
          "name": "Mee Goreng",
          "code": "A001",
          "description": "Delicious Mee Goreng",
          "price": null,
          "stamps_to_redeem": 20,
          "membership": "Blue",
          "merchant": 1,
          "image_url": "foo.png",
          "landscape_url": "foo-landscape.png",
          "extra_data": {
            "SKU": "A001SKU"
          },
          "redeemable": true,
          "terms": "",
          "is_visible": true,
          "is_featured": false,
          "start_date": "2015-12-01",
          "end_date": "2018-12-31"
        },
        {
          "id": 2,
          "name": "Curry Chicken",
          "code": "A002",
          "description": "Delicious Curry Chicken",
          "price": null,
          "stamps_to_redeem": 60,
          "membership": "Blue",
          "merchant": 1,
          "image_url": "foo.png",
          "landscape_url": "foo-landscape.png",
          "extra_data": {},
          "redeemable": false,
          "terms": "",
          "is_visible": true,
          "is_featured": false,
          "start_date": "2015-12-01",
          "end_date": "2018-12-31"
        }
      ],
      "vouchers": [
        {
          "id": 9,
          "code": "BD0201",
          "type": "voucher 1",
          "quantity": 1,
          "notes": "",
          "start_date": "2016-12-01",
          "end_date": "2018-12-31",
          "template": {
            "id": 1,
            "name": "Voucher Template 1",
            "type": 1,
            "description": "",
            "short_description": "",
            "image_url": "foo.png",
            "landscape_url": "foo-landscape.png",
            "instructions": "",
            "terms_and_conditions": "",
            "merchant_code": "ABCDE"
          }
        },
        {
          "id": 10,
          "code": "P010",
          "type": "voucher 2",
          "quantity": 2,
          "notes": "",
          "start_date": "2016-12-01",
          "end_date": "2018-12-31",
          "template": {
            "id": 2,
            "name": "10 Year celebration promo",
            "type": 2,
            "description": "",
            "short_description": "",
            "image_url": "foo.png",
            "landscape_url": "foo-landscape.png",
            "instructions": "Sign up at Stamps and get Free product A",
            "terms_and_conditions": "",
            "merchant_code": "EFCHG"
          }
        }
      ],
      "customer": {
          "id": 114807,
          "stamps": 18,
          "membership_status": "Blue"
      }
    }
