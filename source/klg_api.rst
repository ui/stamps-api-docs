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
name               Yes         Name
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


5. Complete Registration and Merge Legacy Member
====================================
| URL endpoint: https://stamps.co.id/api/klg/memberships/register
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

========================= =========== =========================
Parameter                 Required    Description
========================= =========== =========================
token                     Yes         Authentication string
name                      Yes         Name
email                     No          Email
mobile_number             No          Mobile number
gender                    No          Gender ("male" or "female")
address                   No          Address
birthday                  No          Birthday (with format YYYY-MM-DD)
store                     No          Registering store ID
referral_code             No          Referal code used to register customer
generate_default_password No          Boolean, whether to generate a random, default password for the member, defaults to `true`
registering_employee_code No          String indicating employee code, will create a new employee if not exists
district                  No          District ID
marital_status            No          Marital status mapping can be seen :ref:`here <Marital Status Mapping>`
password                  Yes         User password
pin                       Yes         User pin
confirm_pin               Yes         User pin confirmation
legacy_member             No          Legacy member identifier to merge with
legacy_merchant_id        No          Legacy member merchant ID
========================= =========== =========================


Example of API call request using cURL

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/klg/memberships/register -i -d '{ "token": "secret", "password": "password", "pin": "123123", "confirm_pin": "123123" }'


B. Response
-----------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
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
        "id": "620",
        "name": "John Doe",
        "gender": "male",
        "address": "Jalan Anggrek No. 1",
        "is_active": true,
        "email": "johndoe@example.com",
        "picture_url": "https://media.stamps.co.id/thumb/profile_photos/2014/4/17/483ccddd-9aea-44d2-bbc4-6aa71f51fb2a_size_80.png",
        "birthday": "1993-05-30",
        "phone": "+6285567146065",
        "postal_code": "10310",
        "protected_redemption": false,
        "has_incorrect_email": true,
        "marital_status": 1,
        "religion": 1,
        "wedding_date": null,
        "id_number": null,
        "id_card_file_name": "",
        "phone_is_verified": false,
        "email_is_verified": false,
        "is_anonymized": false,
        "has_pin": false,
        "pin_is_blocked": false,
        "has_password": true,
        "notes": "",
        "referral_code": "GYHTLIY9",
        "registration_status": "Full",
        "location": {
            "district": {
                "id": 1,
                "name": "Kebayoran Baru"
            },
            "regency": {
                "id": 1,
                "name": "Jakarta Selatan"
            },
            "province": {
                "id": 1,
                "name": "DKI Jakarta"
            }
        },
        "membership": {
            "tags": [],
            "status": 0,
            "status_text": "Silver",
            "level": 0,
            "level_text": "Silver",
            "member_status": "Active",
            "stamps": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "7J133",
            "start_date": "2022-11-24",
            "created": "2022-11-24",
            "primary_card": {
                "id": 231,
                "number": "RRRB1AKUT0",
                "is_active": true,
                "activated_time": "2022-01-20 10:00:00"
            }
        },
        "registering_employee_code": "EMP001"
    }


6. Return Transaction Preview
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
stamps_to_add                  No          Stamps to be added by this transaction. If specified, this overrides system's calculation of the number of Stamps that will be added or deducted from this transaction.
stamps_to_deduct               No          Stamps to be deducted manually. If specified, this overrides the number of Stamps that will be deducted from this return.
                                           Can't be sent alongside stamps_to_add.
cancel_redemptions             No          Also cancel redemptions related to original transaction. Default to "false"
issue_voucher                  No          Objects of data used to issue a voucher. Contains ``template_id`` and ``value`` (optional).
deactivate_payment_vouchers    No          Also deactivate unredeemed payment vouchers when set to `true`. Default is `true`.
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


7. Add a return transaction
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
deactivate_payment_vouchers    No          Also deactivate unredeemed payment vouchers when set to `true`. Default is `true`.
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


8. Adding Voucher Redemption
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


9. Webhook Security
=======================
Webhook from STAMPS will return the siganture inside X-Stamps-Signature header.

You should always verify that the webhook's payload matches the signature. To verify the signature against the payload:
    - Strip all whitespace from the `payload`.
    - Encode the resulting `payload` string with the HMAC algorithm, using your secret key as the key and SHA256 as the hashing algorithm.
    - Compare and make sure that the result matches the given `signature`

Example with "your_secret_key" as the secret key:

.. code-block :: json

    {
        "from": "test@gmail.com",
        "to": "test2@gmail.com",
        "membership": {
            "level": 0,
            "level_text": "Silver",
            "stamps": 0,
            "balance": 0,
            "is_blocked": false,
            "referral_code": "KKJ21",
            "start_date": "2023-02-13",
            "created": "2023-02-13",
            "status": "Active",
            "primary_card": {
                "id": 3713,
                "number": "RRR123456",
                "is_active": true,
                "activated_time": null
            }
        },
        "user": {
            "id": 2824,
            "name": "test",
            "gender": "f",
            "is_active": true,
            "email": "test2@gmail.com",
            "birthday": null,
            "picture_url": null,
            "phone": "+6287876544322",
            "has_incorrect_email": false,
            "has_incorrect_phone": false,
            "has_incorrect_wa_number": false,
            "phone_is_verified": true,
            "email_is_verified": true,
            "referral_code": "TESTXYZ",
            "registration_status": "Full",
            "member_ids": [
                "RRR123456"
            ]
        }
    }

Python code example:

.. code-block :: python

    import hashlib
    import hmac
    import json


    def verify_signature(request) -> bool:
        minified_payload = json.dumps(request.data, separators=[",", ":"])
        signed_payload = hmac.new("your_secret_key".encode(), minified_payload.encode(), hashlib.sha256)
        signature = request.headers["X-Stamps-Signature"]
        return hmac.compare_digest(signature, signed_payload.hexdigest())


8. Adding a Transaction with Redemptions
=======================
| URL endpoint: https://stamps.co.id/api/klg/transactions/add-with-redemptions
| Allowed method: POST
| Requires authentication: Yes


A. Request
-----------------------------

Adding a transaction with redemptions requires you to send a POST request to the endpoint with the following parameters:
NOTE: This endpoint also accept blocked membership but won't get any stamps and can not add any redemptions.

=========================== =========== =======================
Parameter                   Required    Description
=========================== =========== =======================
token                       Yes         Authentication string
user                        No          Email address / Member ID indicating customer.
                                        Leaving this empty creates an ``open`` transaction.
store                       Yes         A number (id) indicating store where transaction
                                        is created
invoice_number              Yes         POS transaction number (must be unique daily)
total_value                 Yes         A number indicating transaction's grand total
number_of_people            Yes         An integer indicating the number of people involved in transaction
created                     Yes         ISO 8601 date time format to indicate transaction's
                                        created date
                                        (e.g. 2013-02-15T13:01:01+07)
sub_total                   No          A number indicating transaction subtotal
discount                    No          A number indicating transaction discount (in Rp.)
service_charge              No          A number indicating service charge (in Rp.)
tax                         No          A number indicating transaction tax (in Rp.)
channel                     No          Channel of a transaction, for channel mapping, see table below
type                        No          The type of prepared transactions, for type mapping, see table below
items                       No          List of items containing product name, quantity, subtotal,
                                        stamps_subtotal (optional) & eligible_for_stamps (optional).
                                        ``price`` is the combined price of products (qty * unit price),
                                        ``stamps_subtotal`` is the combined stamps of products (qty * unit stamps),
                                        this field is optional.
                                        ``eligible_for_stamps`` is boolean value to determine whether the item should be included in Stamps Calculation. Defaults to ``true``.
payments                    No          List of payments object containing value, payment_method, and
                                        eligible_for_membership(optional).
                                        ``value`` is the amount of payment
                                        ``payment_method`` is the method used for payment
                                        ``eligible_for_membership`` whether this payment is used for member's status/level changes.
                                        This field is optional. Default to true if not provided(can be configured later).
stamps                      No          A number indicating custom stamps
require_email_notification  No          A boolean indicating send transaction to email if customer can retrieve email
employee_code               No          Employee code of sender employee
extra_data                  No          Additional data for further processing
reward_redemptions          No          List of reward objects that want to be redeemed. Contains ``request_id``, ``reward``, and ``stamps`` (required if reward type is flexible reward). ``reward`` field can be filled with either reward ID (integer, i.e. ``1``) or reward code (string, i.e. ``REWARD1``)
voucher_redemptions         No          List of voucher objects that want to be redeemed. Contains ``request_id`` and ``voucher_code``
=========================== =========== =======================

Channel Mapping

=================== ===========
Code                Description
=================== ===========
1                   Mobile App
2                   POS
3                   Kiosk
4                   Web
5                   Android
6                   iOS
7                   Call Center
8                   GrabFood
9                   GoFood
=================== ===========



Type Mapping

=================== ===========
Code                Description
=================== ===========
1                   Delivery
2                   Dine-in
3                   Take out
4                   E-Commerce
5                   Pickup
=================== ===========



Here's an example of how the API call might look like in JSON format:

.. code-block:: javascript

    {
       "token": "secret",
       "user": "customer@stamps.co.id",
       "stamps": 10,
       "store": 32,
       "invoice_number": "my_invoice_number",
       "sub_total": 45000,
       "total_value": 50000,
       "number_of_people": 8,
       "tax": 5000,
       "channel": 1,
       "require_email_notification": False,
       "employee_code": "employee_code",
       "type": 2,
       "created": "2013-02-15T13:01:01+07",
       "extra_data": {
          "employee_name": "Stamps Employee",
          "order_number": "order_number"
       }
       "items": [
          {
             "product_name": "Cappucino",
             "quantity": 2,
             "subtotal": 10000,
             "stamps_subtotal": 4
          },
          {
             "product_name": "Iced Tea",
             "quantity": 4,
             "subtotal": 5000,
             "stamps_subtotal": 4,
             "eligible_for_stamps": False
          }
       ],
       "payments": [
          {
            "value": 30000,
            "payment_method": 10
          },
          {
            "value": 20000,
            "payment_method": 43,
            "eligible_for_membership": false
          }
       ],
       "reward_redemptions": [
          {
            "request_id": "request-id-1",
            "reward": 1
          },
          {
            "request_id": "request-id-1",
            "reward": "REWARDCODE"
          },
          {
            "request_id": "request-id-1",
            "reward": 1,
            "stamps": 10,
          }
          {
            "request_id": "request-id-1",
            "reward": "REWARDCODE",
            "stamps": 10,
          }
       ],
       "voucher_redemptions": [
          {
            "request_id": "request-id-1",
            "voucher_code": "VOUCHERCODE"
          }
       ]
    }


Example of API call request using cURL (JSON). To avoid HTTP 100 Continue, please specify "Expect:" as a header.

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" -H "Expect:" https://stamps.co.id/api/klg/transactions/add-with-redemptions -i -d '{ "token": "secret", "created": "2017-03-30T07:01:01+07", "user": "customer@stamps.co.id", "store": 422, "number_of_people": 8, "tax":5000, "channel":1, "type":2, "invoice_number": "invoice_1", "total_value": 50000, "items": [{"product_name": "Cappucino", "quantity": 2, "subtotal": 10000}, {"product_name": "Iced Tea", "quantity": 4, "subtotal": 5000}]}, "payments": [{"value": 30000, "payment_method": 10}, {"value": 20000, "payment_method": 43, "eligible_for_membership": false}], "reward_redemptions": [ { "request_id": "request-id-1", "reward": 1 }, { "request_id": "request-id-1", "reward": "REWARDCODE" }, { "request_id": "request-id-1", "reward": 1, "stamps": 10, } { "request_id": "request-id-1", "reward": "REWARDCODE", "stamps": 10, } ], "voucher_redemptions": [ { "request_id": "request-id-1", "voucher_code": "VOUCHERCODE" } ]'

B. Response
-----------------------------

In response to this API call, Stamps will reply with the following data in JSON:

=================== ==================
Variable            Description
=================== ==================
transaction         Stamps transaction information
                    that is successfully created.
                    Contains id, value, number_of_people, discount and stamps_earned.
membership          Contains membership data.
                    Contains ``tags``, ``status``, ``status_text``, ``stamps``, ``balance``,
                    ``is_blocked``, ``referral_code``, ``start_date``, and ``created``
detail              Description of error (if any)
validation_errors   Errors encountered when parsing data (if any)
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
500, 502, 503, 504  Something went wrong on Stamps' server
=================== ==============================

Below are a few examples responses on successful API calls.


If transaction is successful(JSON):

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "membership": {
        "tags": [],
        "status": 10,
        "status_text": "Blue",
        "stamps": 10,
        "balance": 20,
        "is_blocked": false,
        "referral_code": "asd",
        "start_date": "2020-01-01",
        "created": "2020-01-01",
      },
      "transaction": {
        "stamps_earned": 5,
        "id": 2374815,
        "value": 50000.0,
        "number_of_people": 8,
        "discount": 5000.0
      }
    }


When some fields don't validate (JSON):

.. code-block :: bash

    HTTP/1.0 400 BAD REQUEST
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]


    {"detail": "Your transaction cannot be completed due to the following error(s)", "errors": [{"subtotal": "This field is required."}, {"invoice_number": "Store does not exist"}]}


If HTTP is used instead of HTTPS:

.. code-block :: bash

    HTTP/1.0 403 FORBIDDEN
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
     [Redacted Header]

    {"detail": "Please use https instead of http"}


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
====== ============
Code   Description
====== ============
2      POS
3      Kiosk
4      Web
5      Android
6      iOS
====== ============

Marital Status Mapping
^^^^^^^^^^^^^^^^^^^^^^
====== ============
Code   Description
====== ============
1      Single
2      Married
3      Divorced
4      Widowed
5      Others
====== ============
