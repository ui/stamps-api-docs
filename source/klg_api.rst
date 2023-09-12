************************************
Custom KLG API
************************************


1. Get Duplicate Legacy Members
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/check-duplicates-with-pin
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------
You can get unmerged legacy memberships that are potentially duplicate to a legacy member with this API.

============     =========== =========================
Parameter        Required    Description
============     =========== =========================
token            Yes         Authentication token in string
user             Yes         Legacy membership's email or mobile number
pin              Yes         Legacy mebership's PIN
============     =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/legacy/check-duplicates-with-pin?token=123&user=example@stamps.com&pin=123456'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
legacy_members      An array of legacy member objects
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: GET
      [Redacted Header]

    {
      "legacy_members": [
        {
          "member_id": "322145",
          "email": "legacy_member@stamps.com",
          "mobile_number": "+62851111222333",
          "merchant": 1,
          "status": 1
        },
        {
          "member_id": "63414",
          "email": "duplicate_member2@stamps.com",
          "mobile_number": "+62851111222444",
          "merchant": 2,
          "status": 1
        },
      ]
    }

2. Merge Legacy Membership Without Pin
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/merge-without-pin
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
You can merge a legacy membership to a stamps membership with this API

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
token            Yes         Authentication string
target_user      Yes         A string indicating customer's email, Member ID, mobile number or primary key ID
legacy_member    Yes         A string indicating legacy member's ID, mobile number or email
merchant_id      Yes         Merchant ID the legacy member is associated with
bonus_stamps     No          Integer, bonus points given to target user's membership
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/legacy/members/merge -i -d '{ "token": "secret", "target_user": 1, "legacy_member": 31245, "merchant_id": 1, "bonus_stamps": 10 }'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
membership          Various information about target user's membership
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "membership": {
        "level": 100,
        "level_text": "Blue",
        "stamps": 410,
        "balance": 150000,
        "is_blocked": false,
        "referral_code": "ABCDE",
        "start_date": "2014-08-08",
        "created": "2014-08-08",
      }
    }

3. Activate Legacy Membership Without Pin
====================================
| URL endpoint: https://stamps.co.id/api/legacy/members/activate-without-pin
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
This API turns a legacy member data into to an active membership.

================ =========== =========================
Parameter        Required    Description
================ =========== =========================
token            Yes         Authentication string
user             Yes         A string indicating legacy member's ID, mobile number or email
merchant_id      Yes         Merchant ID the legacy member is associated with
bonus_stamps     No          Integer, bonus points given to target user's membership
================ =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/legacy/members/activate-without-pin -i -d '{ "token": "secret", "user": 12, "merchant_id": 1, "bonus_stamps": 10 }'



B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                Customer profile data
membership          Various information about active membership
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {
      "user": {
        "id": "123",
        "name": "Customer",
        "gender": "m",
        "address": "Jl MK raya",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "phone": "+62812398712",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1989-10-1",
      },
      "membership": {
        "level": 1,
        "level_text": "Blue",
        "stamps": 100,
        "balance": 0,
        "is_blocked": false,
        "referral_code": "abc123",
        "start_date": "2022-01-01",
        "created": "2022-01-01",
        "primary_card": {
          "id": 1,
          "number": "RRR123456",
          "is_active": true,
          "activated_time": "2022-01-20 10:00:00"
        }
      }
    }


4. Activate Legacy Membership and Complete Profile
====================================
| URL endpoint: https://stamps.co.id/api/klg/legacy/members/activate
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

================== =========== =========================
Parameter          Required    Description
================== =========== =========================
token              Yes         Authentication string
user               Yes         A string indicating legacy member's ID, mobile number or email
merchant_id        Yes         Merchant ID the legacy member is associated with
passkey            No          Legacy member's PIN, required when `with_passkey` is true
with_passkey       Yes         Boolean, whether to check legacy member PIN or not
bonus_stamps       No          Integer, bonus points given to target user's membership
new_password       Yes         User password
pin                Yes         User pin
confirm_pin        Yes         User pin confirmation
email              No          Email
mobile_number      No          Mobile number
gender             No          Gender
address            No          Address
district           No          District ID
phone_is_verified  No          Boolean
email_is_verified  No          Boolean
================== =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/klg/legacy/members/activate -i -d '{ "token": "secret", "user": 12, "merchant_id": 1, "bonus_stamps": 10, "passkey": "", "with_passkey": false, "new_password": "password", "pin": "123123", "confirm_pin": "123123" }'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
user                Customer profile data
membership          Various information about active membership
errors              Errors encountered when processing request (if any)
=================== ==============================


C. Example Response
-------------------

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "user": {
        "id": "123",
        "name": "Customer",
        "gender": "m",
        "address": "Jl MK raya",
        "is_active": true,
        "email": "customer@stamps.co.id",
        "phone": "+62812398712",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1989-10-1",
        "has_incorrect_email": true,
        "has_incorrect_phone": true,
        "has_incorrect_wa_number": true,
        "phone_is_verified": true,
        "email_is_verified": true,
        "referral_code": "ABCDEF",
        "registration_status": "Full"
      },
      "membership": {
        "level": 1,
        "level_text": "Blue",
        "stamps": 100,
        "balance": 0,
        "is_blocked": false,
        "referral_code": "abc123",
        "start_date": "2022-01-01",
        "created": "2022-01-01",
        "primary_card": {
          "id": 1,
          "number": "RRR123456",
          "is_active": true,
          "activated_time": "2022-01-20 10:00:00"
        }
      }
    }


5. Return Transaction Preview
=======================================
| URL endpoint: https://stamps.co.id/api/returns/preview
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------
Preview a return from a transaction.

============================== =========== =============================================================================
Parameter                      Required    Description
============================== =========== =============================================================================
root_invoice_number            Yes         Invoice number of the first original transaction
root_transaction_store         Yes         Store's id or code where the the first transaction happens
root_invoice_date              Yes         First transaction creation date in YYY-mm-dd format(e.g: 2022-08-30)
return_invoice_number          Yes         Invoice number for return transaction
return_created_datetime        No          When the return happens in ISO 8601 format(e.g: 2013-01-15T20:01:01+07).
                                           Default to now
return_store                   Yes         Store's id or code where the return happens
subtotal_delta                 No          Must be provided if the original transaction has subtotal
total_value_delta              Yes         The delta value of transaction's grand total after returned
payments                       No          Must be provided if original transaction has payments.
                                           Payments are list of :ref:`payment objects <Payment Object>`
stamps_to_add                  No          Stamps to be added by this transaction. If specified, this overrides system's calculation of the number of Stamps that will be added or deducted from this transaction.
stamps_to_deduct               No          Stamps to be deducted manually. If specified, this overrides the number of Stamps that will be deducted from this return.
                                           Can't be sent alongside stamps_to_add.
items                          Yes         Which items are returned. Items are list of :ref:`item objects<Item Object>`
============================== =========== =============================================================================

Example of API call request using cURL

.. code-block :: bash

    curl --location --request POST 'https://stamps.co.id/api/returns/preview' \
    --header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjYxOTMwNjg2LCJpYXQiOjE2NjE4NDQyODYsImp0aSI6IjZlM2U0ZGU0MzZkYzRjNDZhNGJhMjRkZWE2MjM0N2VjIiwidXNlcl9pZCI6MSwibWVyY2hhbnRfaWQiOjF9.brgNBzeuPmOV6ECP5WpwJJlQ6MQZ1zACHYx1YiW33AM' \
    --header 'Content-Type: application/json' \
    --header 'Cookie: csrftoken=FAc0E8TCQSCqCKhNNH62Pr3KTFgfemz2DMPWkjdSkD68VJYKda38emJi8GykuSgd; sessionid=sl07y2ektnrikw4bddkr4kndr482qms4' \
    --data-raw ' {
                "root_invoice_number": "6288988812712621",
                "root_transaction_store": 3,
                "root_invoice_date": "2022-08-30",
                "return_invoice_number": "6288988812712621.1",
                "total_value_delta": -15000,
                "return_store": 3,
                "items": [
                    {
                        "product_name": "tea",
                        "quantity": -1,
                        "subtotal": -15000
                    }
                ],
                "payments": [
                    {
                        "payment_method": "1300",
                        "value": -15000
                    }
                ]
            }'

B. Response Data
----------------

Stamps responds to this API call with the following data (in JSON):

==================== =============================================================================================
Variable             Description
==================== =============================================================================================
user                 Information about this transaction's user
membership           Information about the user's membership on the transaction's merchant
root_transaction     Information about the first original transaction
original_transaction Information about the previous transaction
modified_transaction Information about the new transaction after return happens
modication           Information about the :ref:`modified data <Modification Object>` of the original transaction
returnable_vouchers  Information about what vouchers will be returned
==================== =============================================================================================


C. Response Headers
-------------------

=================== =======================================================================
Code                Description
=================== =======================================================================
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
=================== =======================================================================


D. Examples
-----------

On a successful API call:

.. code-block :: bash

    {
        "user": {
            "id": "2845532",
            "name": "Marsha Test",
            "gender": "male",
            "address": "",
            "is_active": true,
            "email": "marshadouble@test.com",
            "picture_url": null,
            "birthday": "1988-04-23",
            "phone": "+628898881212",
            "postal_code": "",
            "protected_redemption": false,
            "has_incorrect_email": false,
            "marital_status": null,
            "religion": null,
            "wedding_date": null,
            "id_number": null,
            "id_card_file_name": "",
            "phone_is_verified": false,
            "email_is_verified": false
        },
        "membership": {
            "tags": [],
            "status": 100,
            "status_text": "Blue",
            "stamps": 13,
            "stamps_owed": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "AL6J3A9",
            "start_date": "2022-08-03",
            "created": "2022-08-03"
        },
        "root_transaction": {
            "id": 26091,
            "value": 150000.0,
            "stamps_earned": 25,
            "number_of_people": null,
            "discount": null,
            "subtotal": null,
            "items": [
                {
                    "id": 3670,
                    "quantity": 10.0,
                    "subtotal": 150000.0,
                    "price_per_unit": null,
                    "product": {
                        "id": 3,
                        "name": "tea"
                    }
                }
            ],
            "payments": [
                {
                    "id": 78,
                    "value": 150000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "original_transaction": {
            "id": 26091,
            "value": 150000.0,
            "stamps_earned": 25,
            "number_of_people": null,
            "discount": null,
            "subtotal": null,
            "items": [
                {
                    "id": 3670,
                    "quantity": 10.0,
                    "subtotal": 150000.0,
                    "price_per_unit": null,
                    "product": {
                        "id": 3,
                        "name": "tea"
                    }
                }
            ],
            "payments": [
                {
                    "id": 78,
                    "value": 150000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "modified_transaction": {
            "id": null,
            "value": 149999.0,
            "stamps_earned": 13,
            "number_of_people": null,
            "discount": null,
            "subtotal": 135000.0,
            "items": [],
            "payments": [
                {
                    "id": null,
                    "value": 135000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "modification": {
            "id": null,
            "created": 1661844369,
            "stamps_delta": -12,
            "stamps_delta_override": 0,
            "stamps_refund_from_payments": 0,
            "total_stamps_delta": -12,
            "subtotal_delta": -15000.0,
            "grand_total_delta": -15000.0
        },
        "returnable_vouchers": [
            {
                "id": 9,
                "code": "R-FWEWQWVV",
                "is_active": true,
                "quantity": 1,
                "value": 50000.0,
                "notes": "",
                "start_date": "2022-12-23",
                "end_date": "2022-12-29",
                "template": {
                    "id": 5,
                    "name": "MULTI VOUCHER",
                    "type": 2,
                    "description": "",
                    "short_description": "",
                    "picture_url": "/media/thumb/voucher_templates/2022/12/9/uploaded_image_2022_12_09_03_38_21_252743_size_400.webp",
                    "landscape_picture_url": null,
                    "instructions": "",
                    "terms_and_conditions": "",
                    "usable_in_merchant_ids": [
                        1,
                        2
                    ],
                    "merchant_code": "",
                    "extra_data": null,
                    "html_terms_and_conditions": null
                },
                "terms_and_conditions": "",
                "html_terms_and_conditions": null
            }
        ]
    }

On an invalid request:

.. code-block :: bash

    {
        "detail": "root_transaction_store: No store with given identifier",
        "error_message": "root_transaction_store: No store with given identifier",
        "error_code": "invalid_store",
        "errors": {
            "root_transaction_store": "No store with given identifier"
        }
    }


6. Add a return transaction
=======================================
| URL endpoint: https://stamps.co.id/api/returns/add
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

============================== =========== ==================================================================================================================
Parameter                      Required    Description
============================== =========== ==================================================================================================================
root_invoice_number            Yes         Invoice number of the first original transaction
root_transaction_store         Yes         Store's id or code where the the first transaction happens
root_invoice_date              Yes         First transaction creation date in YYY-mm-dd format(e.g: 2022-08-30)
return_invoice_number          Yes         Invoice number for return transaction
return_created_datetime        No          When the return happens in ISO 8601 format(e.g: 2013-01-15T20:01:01+07).
                                           Default to now
return_store                   Yes         Store's id or code where the return happens
subtotal_delta                 No          Must be provided if the original transaction has subtotal
total_value_delta              Yes         The delta value of transaction's grand total after returned
payments                       No          Must be provided if original transaction has payments.
                                           Payments are list of :ref:`payment objects <Payment Object>`
items                          Yes         Which items are returned. Items are list of :ref:`item objects<Item Object>`
stamps_to_add                  No          Stamps to be added by this transaction. If specified, this overrides system's calculation of the number of Stamps that will be added or deducted from this transaction.
stamps_to_deduct               No          Stamps to be deducted manually. If specified, this overrides the number of Stamps that will be deducted from this return.
                                           Can't be sent alongside stamps_to_add.
cancel_redemptions             No          Also cancel redemptions related to original transaction. Default to "false"
issue_voucher                  No          Objects of data used to issue a voucher. Contains ``template_id`` and ``value`` (optional).
============================== =========== ==================================================================================================================

Example of API call request using cURL

.. code-block :: bash

    curl --location --request POST 'https://stamps.co.id/api/returns/add' \
    --header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjYxOTMwNjg2LCJpYXQiOjE2NjE4NDQyODYsImp0aSI6IjZlM2U0ZGU0MzZkYzRjNDZhNGJhMjRkZWE2MjM0N2VjIiwidXNlcl9pZCI6MSwibWVyY2hhbnRfaWQiOjF9.brgNBzeuPmOV6ECP5WpwJJlQ6MQZ1zACHYx1YiW33AM' \
    --header 'Content-Type: application/json' \
    --header 'Cookie: csrftoken=FAc0E8TCQSCqCKhNNH62Pr3KTFgfemz2DMPWkjdSkD68VJYKda38emJi8GykuSgd; sessionid=sl07y2ektnrikw4bddkr4kndr482qms4' \
    --data-raw ' {
                "root_invoice_number": "6288988812712621",
                "root_transaction_store": 3,
                "root_invoice_date": "2022-08-30",
                "return_invoice_number": "6288988812712621.1",
                "total_value_delta": -15000,
                "return_store": 3,
                "items": [
                    {
                        "product_name": "tea",
                        "quantity": -1,
                        "subtotal": -15000
                    }
                ],
                "payments": [
                    {
                        "payment_method": "1300",
                        "value": -15000
                    }
                ],
                "issue_voucher": {
                    "template_id": 1,
                    "value": 10000
                }
            }'

B. Response Data
----------------

Stamps responds to this API call with the following data (in JSON):

==================== ===========================================================================
Variable             Description
==================== ===========================================================================
user                 Information about this transaction's user
membership           Information about the user's membership on the transaction's merchant
root_transaction     Information about the first original transaction
original_transaction Information about the previous transaction
modified_transaction Information about the new transaction after return happens
modification           Information about the :ref:`modified data <Modification Object>` of the original transaction
returned_vouchers    Information about what vouchers are returned
==================== ===========================================================================


C. Examples
-----------

On a successful API call:

.. code-block :: bash

    {
        "user": {
            "id": "2845532",
            "name": "Marsha Test",
            "gender": "male",
            "address": "",
            "is_active": true,
            "email": "marshadouble@test.com",
            "picture_url": null,
            "birthday": "1988-04-23",
            "phone": "+628898881212",
            "postal_code": "",
            "protected_redemption": false,
            "has_incorrect_email": false,
            "marital_status": null,
            "religion": null,
            "wedding_date": null,
            "id_number": null,
            "id_card_file_name": "",
            "phone_is_verified": false,
            "email_is_verified": false
        },
        "membership": {
            "tags": [],
            "status": 100,
            "status_text": "Blue",
            "stamps": 13,
            "stamps_owed": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "AL6J3A9",
            "start_date": "2022-08-03",
            "created": "2022-08-03"
        },
        "root_transaction": {
            "id": 26091,
            "value": 150000.0,
            "stamps_earned": 25,
            "number_of_people": null,
            "discount": null,
            "subtotal": null,
            "items": [
                {
                    "id": 3670,
                    "quantity": 10.0,
                    "subtotal": 150000.0,
                    "price_per_unit": null,
                    "product": {
                        "id": 3,
                        "name": "tea"
                    }
                }
            ],
            "payments": [
                {
                    "id": 78,
                    "value": 150000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "original_transaction": {
            "id": 26091,
            "value": 150000.0,
            "stamps_earned": 25,
            "number_of_people": null,
            "discount": null,
            "subtotal": null,
            "items": [
                {
                    "id": 3670,
                    "quantity": 10.0,
                    "subtotal": 150000.0,
                    "price_per_unit": null,
                    "product": {
                        "id": 3,
                        "name": "tea"
                    }
                }
            ],
            "payments": [
                {
                    "id": 78,
                    "value": 150000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "modified_transaction": {
            "id": 26092,
            "value": 149999.0,
            "stamps_earned": 13,
            "number_of_people": null,
            "discount": null,
            "subtotal": 135000.0,
            "items": [],
            "payments": [
                {
                    "id": 79,
                    "value": 135000.0,
                    "eligible_for_stamps": true,
                    "payment_method_code": "1300"
                }
            ]
        },
        "modification": {
            "id": 1,
            "created": 1661844369,
            "stamps_delta": -12,
            "stamps_delta_override": 0,
            "stamps_refund_from_payments": 0,
            "total_stamps_delta": -12,
            "subtotal_delta": -15000.0,
            "grand_total_delta": -15000.0
        },
        "issued_voucher": {
            "id": 1,
            "code": "VC-ABC",
            "is_active": true,
            "quantity": 1,
            "value": 200,
            "notes": "",
            "start_date": "2022-03-28",
            "end_date": "2022-04-28",
            "template": {
                "id": 1,
                "name": "March Surprise Voucher",
                "type": 1,
                "description": "Get 50% off on your next purchase in Lippo Mall Kemang Store",
                "short_description": "Get 50% off on your next purchase",
                "picture_url": "foo.png",
                "landscape_picture_url": "foo_landscape.png",
                "instructions": "Show this voucher to the cashier",
                "terms_and_conditions": "Valid until 28 April 2022 with minimum purchase of Rp 100.000",
                "usable_in_merchant_ids": [1, 2, 3],
                "merchant_code": "M-ABC",
                "extra_data": null,
            }
        },
        "returned_vouchers": [
            {
                "id": 9,
                "code": "R-ZLT2ULER",
                "is_active": true,
                "quantity": 1,
                "value": 50000.0,
                "notes": "",
                "start_date": "2022-12-23",
                "end_date": "2022-12-29",
                "template": {
                    "id": 5,
                    "name": "MULTI VOUCHER",
                    "type": 2,
                    "description": "",
                    "short_description": "",
                    "picture_url": "/media/thumb/voucher_templates/2022/12/9/uploaded_image_2022_12_09_03_38_21_252743_size_400.webp",
                    "landscape_picture_url": null,
                    "instructions": "",
                    "terms_and_conditions": "",
                    "usable_in_merchant_ids": [
                        1,
                        2
                    ],
                    "merchant_code": "",
                    "extra_data": null,
                    "html_terms_and_conditions": null
                },
                "terms_and_conditions": "",
                "html_terms_and_conditions": null
            }
        ]
    }

On an invalid request:

.. code-block :: bash

    {
        "detail": "root_transaction_store: No store with given identifier",
        "error_message": "root_transaction_store: No store with given identifier",
        "error_code": "invalid_store",
        "errors": {
            "root_transaction_store": "No store with given identifier"
        }
    }


7. Adding Voucher Redemption
======================

| URL endpoint: https://stamps.co.id/api/klg/redemptions/odi-redeem-voucher
| Allowed method: POST
| Require authentication: Yes

A. Parameters
-------------
You can initiate a voucher redemption by calling the API with these parameters.

=============== ========= =========================
Parameter       Required  Description
=============== ========= =========================
token           Yes       Authentication string
user            Yes       A string indicating customer's email or Member ID
voucher         Yes       Voucher code of the redeemed voucher
store           Yes       Merchant's store identifier where redemption is initiated
invoice_number  No        POS invoice number
channel         No        Channel mapping can be seen :ref:`here <Channel Mapping>`
request_id      No        This field is needed if PIN authorization is enabled
extra_data      Yes       JSON object containing "vouchers" object. Store name as the key, and voucher value as the value.
=============== ========= =========================

Here's an example of how the API call might look like in JSON format with specified voucher.

.. code-block :: bash

    {
        "token": "abc",
        "user": "customer@stamps.co.id",
        "store": 32,
        "voucher": 1,
        "invoice_number": "POS-1020123",
        "extra_data": {
            "vouchers": {
                "A001": 10000,
                "I002": 40000
            }
        }
    }

API call example:

.. code-block :: bash

    $ curl --location --request POST 'https://stamps.co.id/api/klg/redemptions/odi-redeem-voucher' \
    --header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjYxOTMwNjg2LCJpYXQiOjE2NjE4NDQyODYsImp0aSI6IjZlM2U0ZGU0MzZkYzRjNDZhNGJhMjRkZWE2MjM0N2VjIiwidXNlcl9pZCI6MSwibWVyY2hhbnRfaWQiOjF9.brgNBzeuPmOV6ECP5WpwJJlQ6MQZ1zACHYx1YiW33AM' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "user": "customer@stamps.co.id",
        "store": 32,
        "voucher": "ABC",
        "extra_data": {
            "vouchers": {
                "A001": 10000,
                "I002": 40000
            }
        }
    }'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
redemption          Redemption information which is
                    successfully created.
                    Contains id, reward, and stamps_used
membership          Customer information after successful
                    redemption. Contains id and stamps_remaining.
voucher             Voucher used in redemption
errors              Errors encountered when processing request (if any)
=================== ==============================

C. Response Headers
-------------------

Depending on the request, responses may return these status codes:

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
405                 HTTP method not allowed
500, 502, 503, 504  Server Errors - something is wrong on Stamps' end
=================== ==============================

D. Example Response
-------------------

On successful redemption:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]
    {
       "redemption": {
            "id": 2,
            "reward": "Discount Rp 100,000",
            "stamps_used": 0,
            "extra_data": {
                "discount": "10%"
            }
        },
        "membership": {
            "tags": [],
            "status": 100,
            "stamps": 250,
            "balance": 0,
            "referral_code": "9121682",
            "start_date": "2016-07-25",
            "created": "2016-07-25"
        },
        "voucher": {
            "id": 4,
            "name": "Discount Rp 100,000",
            "code": "PZ633ECV",
            "type": "voucher"
        }
    }

Miscellaneous
------------------------------

Payment Object
^^^^^^^^^^^^^^
============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
payment_method                 Yes         Payment method code
value                          Yes         Nominal of payment, must be negative
voucher_code                   No          Will issue a new voucher with corresponding `value`
============================== =========== ===================================================================

Item Object
^^^^^^^^^^^
============================== =========== ===================================================================
Parameter                      Required    Description
============================== =========== ===================================================================
product_name                   Yes         Product name of the item
quantity                       Yes         Returned quantity, must be negative
subtotal                       Yes         Returned subtotal, must be negative
============================== =========== ===================================================================

Modification Object
^^^^^^^^^^^^^^^^^^^
============================== =========== =============================================================================================
Parameter                      Type        Description
============================== =========== =============================================================================================
id                             int         Modification ID
created                        int         Created time of modification in Unix Timestamp format
stamps_delta                   int         Stamps delta between previous and latest transaction, from default calculation
stamps_delta_override          int         Stamps delta between previous and latest transaction, from stamps_to_add or stamps_to_deduct
stamps_refund_from_payments    int         Refunded stamps from the defined refundable payment methods
total_stamps_delta             int         Total stamps delta from stamps_delta + stamps_delta_override + stamps_refund_from_payments
subtotal_delta                 float       Subtotal delta between previous and latest transaction
grand_total_delta              float       Grand total delta between previous and latest transaction
============================== =========== =============================================================================================

Channel Mapping
^^^^^^^^^^^^^^^
=========== ======
Channel     Value
=========== ======
POS         2
Kiosk       3
Web         4
Android     5
iOS         6
=========== ======
