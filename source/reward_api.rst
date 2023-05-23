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
only_redeemable_in_this_store  No          `true` or `false`. Defaults to `false`.
                                           If `true`, only rewards redeemable in given store will be returned.
include_inactive_vouchers      No          Boolean indicating response include inactive vouchers or not
channel                        No          Integer indicating :ref:`channel <Channel Type>` number to be queried for reward.
last_voucher_id                No          Integer, Specifies the ID of the last voucher from which the API should start retrieving vouchers.
last_reward_id                 No          Integer, Specifies the ID of the last reward from which the API should start retrieving rewards.
per_page                       No          Integer, Determines the number of records to be returned per API call.
============================== =========== ===================================================================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/rewards/?token=abc&user=customer@stamps.co.id&merchant=14&store=1channel=2'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Customer information after successful query. Contains id, stamps_remaining, and status.
rewards             List of rewards available for redemption.
                    Contains id, name, stamps_required, extra_data, image_url, is_visible,
                    :ref:`type <Reward Type>` and redeemable(If user is provided)
vouchers            List of rewards available for redemption by user.
                    Contains  id, name, type, quantity, image_url, extra_data
                    landscape_url, expires_on and constraint :ref:`channels <Channel Type>`.
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
          "expires_on": "13-02-2013 00:00",
          "terms": "input your birthday for get voucher on your birthday",
          "quantity": 1,
          "constraint": {
              "channels": [1, 2]
          },
          "extra_data": {
             "SKU": "PROMO-birthday-20-off"
          },
        },
        {
          "id": 10,
          "name": "10 Year celebration promo",
          "code": "P010",
          "landscape_url": "foo-landscape.png",
          "image_url": "foo.png",
          "type": "promotion 1",
          "expires_on": "24-01-2014 00:00",
          "terms": "sign up at stamps and get Free product A",
          "quantity": 2,
          "extra_data": {},
          "constraint": {
              "channels": [1, 2, 3, 4, 5, 6]
          },
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


Miscellaneous
------------------------------

Channel Type
^^^^^^^^^^^^
=================== ===========
Code                Description
=================== ===========
1                   Mobile app
2                   POS
3                   Kiosk
4                   Web
5                   Android
6                   iOS
=================== ===========

2. Get Reward Detail
=======================================
| URL endpoint: https://stamps.co.id/api/rewards/{reward_code}
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for all available rewards on stamps with optional checking to user's capability to redeem the rewards.

============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
token                          Yes         Authentication string
============================== =========== ===================================================================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/rewards/ABCDE1?token=abc'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
reward              Various reward data
errors              Errors encountered when parsing
                    data (if any)
=================== ==============================


c. Examples
-----------

On a successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "id": 1,
      "name": "Example Reward",
      "stamps_to_redeem": 100,
      "get_absolute_url": "/merchant/rewards/abcde1",
      "is_cross_promo": false,
      "description": "An example description of a reward",
      "redemption_url": "",
      "membership": "Blue",
      "picture_url": "/foo.png",
      "is_active": true,
      "code": "ABCDE1",
      "extra_data": {},
      "available_at": ["store A", "store B"]
    }


Miscellaneous
------------------------------

Reward Type
^^^^^^^^^^^
=================== ===========
Code                Description
=================== ===========
1                   Product
3                   Benefit
4                   Voucher
5                   Flexible Reward
=================== ===========
