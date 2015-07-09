FORMAT: 1A
HOST: https://sandbox-mobile.youandpush.com/

# You 'n Push mobile API

# URL

Production url : [https://mobile.youandpush.com/](https://mobile.youandpush.com/ "Production")  
Sandbox url : [https://sandbox-mobile.youandpush.com/](https://sandbox-mobile.youandpush.com/ "Sandbox")

# Requests Headers

You need your requests to be authenticated using the following HTTP header:

- **API key** : Authorization:ApiKey &lt;your_apikey&gt;

You can find this API key in the You 'n Push back office in your application configuration. You must use a **Mobile** key.

# Group Products
Products related resources of the **Purchase API**

## Products Collection [/v1/application/{applicationID}/installation/{installationID}/products]

### List available Products [GET]

Retrieve all available products for this end-user for a specified list of tags.

Tags are specified in the request using the `tags` parameter: `?tags=tag1,tag2`.

The products shown will be products with no tag or products where all the tags are present in the `tags` parameter.

+ Response 200 (application/json)

        [
            {
                "id": "myId",
                "title": "My new Product",
                "productType": "NORMAL"
            }, 
            {
                "id": "anotherId",
                "title": "One of my other products",
                "productType": "REDUCED"
            }
        ]

+ Response 401

+ Response 403

+ Response 404


# Group Transactions
Transactions related resources of the **Purchase API**

## Transaction [/v1/application/{applicationID}/installation/{installationID}/product/{productId}/transaction]

### Create a Transaction [POST]

Likely in reponse to the POST, you will get AWAITING_VALIDATION for the transaction state.
If that case, the custom values of the product is not sent until the transaction is VALID.

The possible values for the state attribute are:
* VALID
* TEST_VALID (same as VALID but returned when you hit the SANDBOX environment).
* INVALID
* AWAITING_VALIDATION

+ Request (application/json)

        {
            "currency": "EUR",
            "identifier": "1234567890123456",
            "estimatedPrice": 14.99,
            "receipt": "receipt as sent to the device by vendor"
        }


+ Response 201

        {
            "id": "1234567890123456",
            "currency": "EUR",
            "estimatedPrice": 14.99,
            "state": "AWAITING_VALIDATION"
            "product": {
                "id": "1234567890123456",
                "productType": "NORMAL",
        }

## Transaction [/v1/application/{applicationID}/installation/{installationID}/product/{productId}/transaction/{transactionId}]

### Get a Transaction [GET]

+ Response 200 (application/json)

        {
            "id": "1234567890123456",
            "currency": "EUR",
            "estimatedPrice": 14.99,
            "state": "VALID"
            "product": {
                "id": "1234567890123456",
                "productType": "NORMAL",
                "customValues": {
                    "myKey": "myValue",
                    "myOtherKey": "my other value"
                }
            }
        }

+ Response 401 

        Wrong API Key value 

+ Response 403

        The installation doesn't belong to the application.

+ Response 404


# Group Inbox

The Inbox is the place where you will find all your messages

## Inbox [/v1/installations/{installationID}/inbox]

+ Parameters
    + installationID (required, string, `394233b28f7c4213`) ... Installation unique identifier

### Create an Inbox [GET]

The server will directly set the **userID** of your inbox with the one which is set in the installation. Make sure you set it before creating the inbox.

+ Response 201 (application/json)

        {
            "id": "1081d2d305904a20"
        }

# Group Messages

Once you created your Inbox, you can do stuff with your messages

## Messages [/v1/installations/{installationID}/inbox/{inboxID}/messages]

+ Parameters
    + installationID (required, string, `394233b28f7c4213`) ... Installation unique identifier
    + inboxID (required, string, `1081d2d305904a20`) ... Inbox unique identifier

### Messages List [GET]

+ Response 200 (application/json)

        [
            {
                "id": "q9vv9kkn1g1jde0d",
                "read": false,
                "title": "hello ljenkins"
            },
            {
                "id": "ouum72l2nu2vek2k",
                "read": false,
                "title": "my title",
                "text": "my text"
            }
        ]
        
### Create a Message [POST]

You might want to synchronize a local message with your You 'n Push Inbox

+ Request (application/json)

        {
            "title": "hello",
            "text": "yo"
        }
        
+ Response 201


## Messages [/v1/installations/{installationID}/inbox/{inboxID}/messages/{messageID}]

+ Parameters
    + installationID (required, string, `394233b28f7c4213`) ... Installation unique identifier
    + inboxID (required, string, `1081d2d305904a20`) ... Inbox unique identifier
    + messageID (required, string, `ouum72l2nu2vek2k`) ... Message unique identifier
    

### Message Detail [GET]

+ Response 200 (application/json)

        {
            "id": "ouum72l2nu2vek2k",
            "read": false,
            "title": "my title",
            "text": "my text"
        }
        
        
### Update a Message [PUT]

Use this route to mark a message as **read**

+ Request (application/json)

        {
            "read": true
        }
        
+ Response 200


