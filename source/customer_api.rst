************************************
Membership API
************************************

1. Getting Member Data
=======================================
| URL endpoint: https://stamps.co.id/api/memberships/status
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

You can query for a customer's data on Stamps .

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
user        Yes         A string indicating customer's email or Member ID
merchant    Yes         Integer indicating merchant ID
=========== =========== =========================

Example of API call request using cURL

.. code-block :: bash

    # Please note that for cURL command you need to escape special characters
    $ curl 'https://stamps.co.id/api/memberships/status?token=abc&user=customer@stamps.co.id&merchant=14'


B. Response Data
----------------

In response to this API call, Stamps will return response with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
stamps              Total stamps amount the
                    particular user has
membership_status   Membership status of the user
is_active           Whether user is registered on Stamps
detail              Description of error (if any)
validation_errors   Errors encountered when parsing
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

    {"stamps": 8917, "membership_status": "Gold"}


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


2. Member Suggestions
=====================
| URL endpoint: https://stamps.co.id/api/memberships/suggestions
| Allowed Method: GET
| Require Authentication: Yes

A. Request
-----------------------------

Manual inputs are time consuming and prone to errors. Member entry interfaces
can be made easier to use by offering autocompletions. Given a sequence of
characters, this API returns a list of possible member matches.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
query       Yes         A string indicating query
                        to be processed for the suggestions API
merchant    Yes         Integer indicating merchant ID
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl 'https://stamps.co.id/api/memberships/suggestions?token=abc&query=steve&merchant=14'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
suggestions         List of suggestions.
                    Contains id, name, stamps, email, and membership
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "suggestions": [
        {
          "membership": "Gold",
          "email": "customer_gold@stamps.co.id",
          "stamps": 100,
          "id": 12,
          "name": "Customer Gold"
        },
        {
          "membership": "Blue",
          "email": "blue_customer@stamps.co.id",
          "stamps": 15,
          "id": 13,
          "name": "Customer Blue"
        }
      ]
    }


3. Registration
===============
| URL endpoint: https://stamps.co.id/api/memberships/register
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to register your customer through Point of Sales
or other websites. On successful redemption, Stamps will send an email
containing an automatically generated password.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
merchant    Yes         Integer indicating merchant ID
name        Yes         Customer's name
email       Yes         Customer's email
birthday    Yes         Customer's birthday (with format YYYY-MM-DD)
gender      Yes         Customer's gender ("male" or "female")
member_id   No          Customer's member (card) id
phone       No          Customer's phone number
address     No          Customer's address
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/memberships/register -i -d '{ "token": "secret", "name": "me", "email": "me@mail.com", "member_id": "123412341234", "phone": "+62215600010", "birthday": "1991-10-19", "gender": "Female", "merchant": 14, "address": "221b Baker Street"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "customer": {
        "id": 3,
        "name": "me",
        "email": "me@mail.com",
        "member_id": "123412341234",
        "phone": "0215600010",
        "birthday": "1991-10-19",
        "gender": "Female",
      }
    }



4. Change Member Info
===============
| URL endpoint: https://stamps.co.id/api/memberships/change
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to update your customer's profile through Point of Sales
or other websites.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
merchant    Yes         Integer indicating merchant ID
name        Yes         Customer's name
email       Yes         Customer's email
birthday    Yes         Customer's birthday (with format YYYY-MM-DD)
gender      Yes         Customer's gender ("male" or "female")
member_id   No          Customer's member (card) id
phone       No          Customer's phone number
address     No          Customer's address
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/memberships/change -i -d '{ "token": "secret", "name": "me", "email": "me@mail.com", "member_id": "123412341234", "phone": "+62215600010", "birthday": "1991-10-19", "gender": "Female", "merchant": 14, "address": "221b Baker Street"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]

    {
      "customer": {
        "id": 3,
        "name": "me",
        "email": "me@mail.com",
        "member_id": "123412341234",
        "phone": "0215600010",
        "birthday": "1991-10-19",
        "gender": "Female",
      }
    }



5. Add Stamps
===============
| URL endpoint: https://stamps.co.id/api/memberships/add-stamps
| Allowed Method: POST
| Require Authentication: Yes

A. Request
-----------------------------

You can use this API to add stamps to stamps member.

=========== =========== =========================
Parameter   Required    Description
=========== =========== =========================
token       Yes         Authentication string
merchant    Yes         Integer indicating merchant ID
email       Yes         Customer's email
stamps      Yes         Stamps amount to be awarded
note        No          Reason for stamps addition
=========== =========== =========================

Example of API call request using cURL:

.. code-block :: bash

    $ curl -X POST -H "Content-Type: application/json" https://stamps.co.id/api/memberships/add_stamps -i -d '{ "token": "secret", "email": "me@mail.com", "merchant": 14, "stamps": 12, note: "Extra 20 stamps"}'


B. Response Data
----------------
Stamps responds to this API call with the following data (in JSON):

=================== ==============================
Variable            Description
=================== ==============================
customer            Various customer data
award               Various award data
=================== ==============================


C. Response Codes
-----------------

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

A successful API call:

.. code-block :: bash

    HTTP/1.0 200 OK
    Vary: Accept
    Content-Type: application/json
    Allow: POST, OPTIONS
    [Redacted Header]


    response = {
        "award": {
            "id": 1,
            "stamps":10,
        },
        "customer": {
            "id": 16999,
            "stamps_remaining": 20,
            "status": "Blue"
        }
    }
