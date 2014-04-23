************************************
Booking API
************************************

1. Adding a Booking
===================
| URL endpoint: https://stamps.co.id/api/bookings/add
| Allowed method: POST
| Requires authentication: No


A. Request
----------

You can add a new booking on Stamps by calling the API with these parameters.

=================== =========== =======================
Parameter           Required    Description
=================== =========== =======================
user                Yes         Email address / Member ID indicating customer
store               Yes         A number indicating store for reservation
date                Yes         Desired booking date in YYYY-MM-DD format (e.g: "2014-04-22")
time                Yes         Desired booking time in 24 hour format (e.g: "19:50")
phone               Yes         Customer's mobile number for confirmation purposes
number_of_people    Yes         Number of seats to be reserved
notes               No          Additional notes, comments or message regarding booking request
=================== =========== =======================

Here's an example of how the API data might look like in JSON format:


.. code-block:: javascript

    {
       "user": "customer@stamps.co.id",
       "store": 2,
       "date": "2014-04-22",
       "time": "19:50",
       "phone": "+6281123456",
       "number_of_people": 8,
       "notes": "Prefers non smoking area."
    }


Here's an example of a Booking API call using cURL.

.. code-block :: bash
    
    curl -i -X POST -H "Content-Type: application/json" "http://127.0.0.1:8000/api/bookings/add" -i -d '{"user": "test@stamps.co.id", "store": 1, "number_of_people": 5, "date": "2015-04-22", "time": "18:28", "phone": "+62123123"}'


B. Response
-----------

On a successful API call, Stamps will reply with created booking data in JSON:

=================== ==================
Variable            Description
=================== ==================
Booking             Booking data that was created.
                    Contains id, number of people and other details.
detail              Description of error (if any)
validation_errors   Errors encountered when parsing data (if any)
=================== ==================

Below are a few examples responses on successful API calls.


If transaction is successful(JSON):

.. code-block:: javascript

    HTTP/1.0 200 OK
    Allow: POST, OPTIONS
    Content-Type: application/json
    Date: Wed, 23 Apr 2014 07:02:10 GMT
    Server: WSGIServer/0.1 Python/2.7.4
    Vary: Accept, Cookie

    {
        "booking": {
            "date": "2015-03-02", 
            "id": 11, 
            "number_of_people": 5, 
            "phone": "+622134554", 
            "status": "Requested", 
            "store": 1, 
            "time": "15:15:00"
        }
    }
