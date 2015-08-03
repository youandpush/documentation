FORMAT: 1A
HOST: https://sandbox-api.youandpush.com/

# You 'n Push API

# URL

Production url : [https://api.youandpush.com/](https://api.youandpush.com/ "Production")  
Sandbox url : [https://sandbox-api.youandpush.com/](https://sandbox-api.youandpush.com/ "Sandbox")

# Requests Headers

You need your requests to be authenticated. There are 2 authentications that are possible :

- **Credentials** : Authorization:Basic base64(&lt;your_user&gt;:&lt;your_password&gt;)
- **API key** : Authorization:ApiKey &lt;your_apikey&gt;

# Parameters for Index Requests

## Pagination
Pagination is achieved using the two following parameters:
- **batchSize** : `batchSize=42` to get 42 elements.
- **batch** : `batch=7` to get the 8th batch of **batchSize** elements (starting at batch 0).

Except if otherwise specified, **batch** defaults to 0 and **batchSize** to 20.

## Sorting
You can sort items on a specific field using the **sort** parameter.
For example, using the parameter `sort=name` will cause the returned items to be sorted by ascending name.

You can also sort in descending order by prefixing the field name with **-**.

Example: `sort=-name`

## Query
You can filter the returned items using the **q** parameter.

This parameter takes an url-encoded expression as value.

Expressions are defined by the following grammar:

expr =
- **expr & expr**
- **expr | expr**
- **(expr)**
- **field comparator value**

comparator for numeric fields =
- **<**
- **<=**
- **>**
- **>=**
- **=**
- **!=**

comparator for alphanumeric fields =
- **=**
- **!=**
- **~** (full text search, ex: name~You*Push)

comparator for boolean fields =
- **=**
- **!=**

Examples:
- test=true (q=test%3Dtrue)
- name="You 'n Push" & test=✓ (q=name%3D%22You%20%27n%20Push%22%20%26%20test%3D✓)


# Group Clients
Clients related resources of the **Clients API**

## Clients Collection [/v1/clients]

### List all Clients [GET]

Retrieve all clients the authenticated user can access (as member or contractor)

You can query the following attributes :  
- `address1`  
- `address2`  
- `city`  
- `exportData`  
- `name`  
- `productionFeedbackUrl`  
- `sandboxFeedbackUrl`  
- `state`  
- `zipCode`  
- `default`  
- `country`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=city,state`

+ Response 200 (application/json)

        [
            {
                "id": "95e8fc8592184435",
                "address1": "38 rue des Mathurins",
                "address2": "www.sophiacom.fr",
                "city": "Paris",
                "exportData": false,
                "name": "Sophiacom",
                "productionFeedbackUrl": "http://sophiacom-integmobile/sophiacom",
                "sandboxFeedbackUrl": "http://sophiacom-integmobile-sandbox/sophiacom",
                "state": "000000",
                "zipCode": "75008",
                "default": true,
                "country":
                {
                    "id": "FR",
                    "iso3Code": "FRA",
                    "name": "FRANCE"
                }
            }, 
            {
                "id": "95e8fc8592184276",
                "address1": "39 rue des Mathurins",
                "address2": "",
                "city": "Paris",
                "exportData": true,
                "name": "NextToUs",
                "productionFeedbackUrl": "http://nexttous-integmobile/sophiacom",
                "sandboxFeedbackUrl": "http://nexttous-integmobile-sandbox/sophiacom",
                "state": "000000",
                "zipCode": "75008",
                "default: false
                "country": 
                {
                    "id": "FR",
                    "iso3Code": "FRA",
                    "name": "FRANCE"
                }       
            }
        ]

+ Response 401

+ Response 403

+ Response 404

## Client [/v1/clients/{clientID}]

A single Client object with all its details

+ Parameters
    + clientID (required, string, `95e8fc8592184276`) ... Client unique identifier

### Retrieve a Client [GET]

+ Response 200 (application/json)

        {
            "id": "95e8fc8592184276",
            "address1": "39 rue des Mathurins",
            "address2": "",
            "city": "Paris",
            "exportData": true,
            "name": "NextToUs",
            "productionFeedbackUrl": "http://nexttous-integmobile/sophiacom",
            "productionFeedbackUrlEnabled": true,
            "sandboxFeedbackUrl": "http://nexttous-integmobile-sandbox/sophiacom",
            "sandboxFeedbackUrlEnabled": false,
            "state": "000000",
            "zipCode": "75008",
            "default": "true",
            "country": 
            {
                "id": "FR",
                "iso3Code": "FRA",
                "name": "FRANCE"
            }
        }

### Modify a Client [PUT]

+ Request (application/json)

        {
            "address1": "39 rue des Mathurins",
            "address2": "",
            "city": "Paris",
            "exportData": true,
            "name": "NextToUs",
            "productionFeedbackUrl": "http://nexttous-integmobile/sophiacom",
            "productionFeedbackUrlEnabled": true,
            "sandboxFeedbackUrl": "http://nexttous-integmobile-sandbox/sophiacom",
            "sandboxFeedbackUrlEnabled": false,
            "state": "000000",
            "zipCode": "75008",
            "default": "true",
            "countryID": "FR"
        }

+ Response 200


# Group Users
Users related resources of the **Users API**

## Authenticated User Detail [/v1/user]

### Show authenticated user [GET]

Returns 401 if the credentials were wrong (way to test credentials)

Possibility to add the recursive parameter : `?recursive=true` which is going to fill the client object recursively so you can get all the client's projects and applications at the same time.


+ Response 200 (application/json)

        {
            "id": 2,
            "createdAt": 1400061224,
            "updatedAt": 1400061224,
            "lastLoggedAt": 1400061224,
            "email": "kperry@youandpush.com",
            "firstName": "Katy",
            "jabberID": null,
            "lastName": "Perry",
            "login": "katperry",
            "revoked": false,
            "revokedChangedAt": null,
            "clientRoles": [
                {
                    "client": {
                        "id": "95e8fc8592184276"
                    },
                    "role": "user"
                },
                {
                    "client": {
                        "id": "7f99fbc530544b63"
                    },
                    "role": "partner"
                }
            ],
            "language": {
                "id": "fr",
                "name": "French",
                "shortCode": "fr"
            }
        }
        
+ Response 401

+ Response 403

+ Response 404

## Authenticated User Modification [/v1/user/{userID}]

+ Parameters
    + userID (required, string, `f58811b1b98749ac`) ... User unique identifier
    
### Modify the Authenticated User [PUT]

Editable Fields :
- login  
- password  
- firstName  
- lastName  
- email  
- cellPhone  
- jabberID  
- language  
- revoked  

+ Request (application/json)

        {
            "email": "spears@youandpush.com",
            "firstName": "Britney",
            "lastName": "Spears",
            "login": "bspears"
        }

+ Response 200
        
+ Response 401

+ Response 403

+ Response 404

## Users Collection [/v1/clients/{clientID}/users]

+ Parameters
    + clientID (required, string, `7f99fbc530544b63`) ... Client unique identifier
    

### Create a User [POST]

+ Request (application/json)

        {
            "email": "mogeret@youandpush.com",
            "cellPhone": "+33612131415",
            "admin": false,
            "firstName": "Marc",
            "lastName": "Ogeret",
            "login": "marcogeret",
            "jabberID": null,
            "password": "12345678",
            "language": {
                "id": "fr"
            }
        }

+ Response 201 (application/json)
        
        {
            "id": "f58811b1b98749ac"
        }

### List all Users [GET]

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `firstName`  
- `lastName`   
- `login`  
- `lastLoggedAt`  
- `revoked`  
- `language`  
- `jabberID`  
- `cellPhone`  
- `revokedChangedAt`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=lastLoggedAt,login`

+ Response 200 (application/json)

        [
            {
                "id": 1,
                "cellPhone": "+33612131415",
                "createdAt": 1400061224,
                "updatedAt": 1400061224,
                "lastLoggedAt": 1400061224,
                "email": "mlavoine@youandpush.com",
                "firstName": "Marc",
                "lastName": "Lavoine",
                "login": "marclavoine",
                "revoked": false,
                "revokedChangedAt": null,
                "clientRoles": [
                    {
                        "client": {
                            "id": "95e8fc8592184276"
                        },
                        "role": "user"
                    }
                ],
                "language": {
                    "id": "fr",
                    "name": "French",
                    "shortCode": "fr"
                }
            }, 
            {
                "id": 2,
                "createdAt": 1400061224,
                "updatedAt": 1400061224,
                "lastLoggedAt": 1400061224,
                "email": "kperry@youandpush.com",
                "firstName": "Katy",
                "lastName": "Perry",
                "login": "katperry",
                "revoked": true,
                "revokedChangedAt": 1400081224,
                "clientRoles": [
                    {
                        "client": {
                            "id": "7f99fbc530544b63"
                        },
                        "role": "user"
                    }
                ],
                "language": {
                    "id": "en",
                    "name": "English",
                    "shortCode": "en"
                }
            }
        ]

+ Response 401

+ Response 403

+ Response 404


## User [/v1/clients/{clientID}/users/{userID}]
A single User object with all its details

+ Parameters
    + clientID (required, string, `95e8fc8592184276`) ... Client unique identifier
    + userID (required, string, `f58811b1b98749ac`) ... User unique identifier

### Retrieve a User [GET]

+ Response 200 (application/json)

        {
            "id": 2,
            "created": 1434731021495,
            "email": "kperry@youandpush.com",
            "firstName": "Katy",
            "jabberID": null,
            "lastLoggedAt": 1434731021495,
            "lastModified": 1434731021495,
            "lastName": "Perry",
            "login": "katperry",
            "revoked": false,
            "revokedChangedAt": null
        }

### Modify a User [PUT]

+ Request (application/json)

        {
            "email": "davidbowie@youandpush.com",
            "firstName": "David",
            "lastName": "Bowie",
            "jabberID": null
        }

+ Response 200

### Remove a User [DELETE]
+ Response 200

# Group Partners

## Partners [/v1/clients/{clientID}/partners]

+ Parameters
    + clientID (required, string, `7f99fbc530544b63`) ... Client unique identifier
    

### Add a Partner [POST]

To add a partner, you need to precise either the login or the email of the person you want to add.

+ Request (application/json)

        {
            "login": "npaggle",
            "email": "npaggle@fishinc.com"
        }
        
+ Response 201 (application/json)

        {
            "id": "0a79f0d6f8444d73"
        }

### List all Partners [GET]

You can query the following attributes :
- `firstName`  
- `lastName`  
- `login`  
- `email`  
- `company`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=login,company`

- Response 200 (application/json)

        [
            {
                "id": "0a79f0d6f8444d73",
                "login": "npaggle",
                "company": "Fishing Inc"
            },
            {
                "id": "f7baa42697374c1e",
                "login": "ljenkins",
                "company": "Dragons Corp."
            }
        ]

## Partners [/v1/clients/{clientID}/partners/{partnerID}]

+ Parameters
    + clientID (required, string, `7f99fbc530544b63`) ... Client unique identifier
    + partnerID (required, string, `0a79f0d6f8444d73`) ... Client unique identifier
    
### Partner Details [GET]

+ Response 200 (application/json)

        {
            "id": "0a79f0d6f8444d73",
            "login": "npaggle",
            "email": "npaggle@fishinc.com",
            "firstName": "Nat",
            "lastName": "Paggle",
            "Company": "Fishing Inc"
        }

### Delete a Partner [DELETE]

authentication : **Credentials**

+ Response 200


# Group Projects
Projects related resources of the **Projects API**

## Projects Collection [/v1/clients/{clientID}/projects]

+ Parameters
    + clientID (required, string, `7f99fbc530544b63`) ... Client unique identifier


### Create a Project [POST]

+ Request (application/json)

        {
            "name": "YNP Client"
        }

+ Response 201 (application/json)

        {
            "id": "3jjcmr4qbjn41v0g"
        }
        

### List all Projects [GET]

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `comments`  
- `name`   
- `applications`  
- `deletable`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=updatedAt,name`

+ Response 200 (application/json)

        [
            {
                "id": "aucitbeb99rtteor",
                "type": "project",
                "createdAt": 1434960493392,
                "comments": "Applications qui permettent de tester YNP. On va voir.",
                "updatedAt": 1434960493392,
                "name": "YNP Client"
            }, 
            {
                "id": "3jjcmr4qbjn41v0g",
                "type": "project",
                "createdAt": 1434960493392,
                "comments": null,
                "updatedAt": 1434960493392,
                "name": "purchase test project"       
            }
        ]

+ Response 401

+ Response 403

+ Response 404

## Project [/v1/clients/{clientID}/projects/{projectID}]
A single Project object with all its details

+ Parameters
    + clientID (required, string, `7f99fbc530544b63`) ... Client unique identifier
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier

### Retrieve a Project [GET]

+ Response 200 (application/json)

        {
            "id": "3jjcmr4qbjn41v0g",
            "type": "project",
            "created": "2012-09-10T19:17:02Z",
            "comments": null,
            "lastModified": "2012-11-08T19:21:14Z",
            "name": "purchase test project"
        }

### Modify a Project [PUT]

+ Request (application/json)

        {
            "type": "project",
            "comments": null,
            "name": "purchase test project"
        }

+ Response 200
        
### Remove a Project [DELETE]

+ Response 200



# Group Applications
Applications related resources of the **Applications API**

## Applications Collection [/v1/projects/{projectID}/applications]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    
### List all Applications [GET]

authentication : **Credentials**

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `invalidAt`  
- `name`   
- `displayName`  
- `development`
- `platform`  
- `languages`  
- `discriminatorKeysGroups`  
- `productionCertificate`  
- `sandboxCertificate`  
- `gcmApiKey`  
- `pushGateway`  
- `developmentApplicationID`  
- `productionApplicationID`  
- `sharedSecret`  
- `securityIdentifier`  
- `token`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=displayName,platform,development`

+ Response 200 (application/json)

        [
            {
                "id": "bj0nj6i7uindk6u5",
                "displayName": "Windows phone ?",
                "platform": "WIN_PHONE",
                "development": false
            },
            {
                "id": "ujl99ghvalgpllf9",
                "displayName": "This is the Android one",
                "platform": "ANDROID",
                "development": false
            },
            {
                "id": "vhqpteg7m52iuj8b",
                "displayName": "Windows8 test app",
                "platform": "WINDOWS",
                "development": true
            },
            {
                "id": "e9ahnumqnclkl33g",
                "displayName": "My iOS Production application",
                "platform": "IOS",
                "development": false
            }
        ]

+ Response 401

+ Response 403

+ Response 404

### Create an Application [POST]

+ Request (application/json)

        {
            "languages": [
                {
                    "id": "fr",
                },
                {
                    "id": "en",
                }
            ],
            "platform": "IOS",
            "name": "com.youandpush.brandnew",
            "displayName": "This is a new iOS Application",
            "development": false
        }

+ Response 201

## Application [/v1/projects/{projectID}/applications/{applicationID}]
A single Application object with all its details

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    + applicationID (required, string, `lg1nbfk5ksgp68r6`) ... Application unique identifier

### Retrieve an Application [GET]

If the **sharedSecret** is known, the request will return "***" to show that we know it.
The **password** field in iOS certificates works the same.

+ Response 200 (application/json)

        {
            "id": "lg1nbfk5ksgp68r6",
            "createdAt": 1438182659000,
            "updatedAt": 1438182659000,
            "invalidAt": null,
            "displayName": "This is a new iOS Application",
            "platform": "IOS",
            "name": "com.youandpush.brandnew",
            "development": false,
            "languages": [
                {
                    "id": "fr"
                },
                {
                    "id": "en"
                }
            ],
            "discriminatorKeysGroups": {
                "Std": "SHORT",
                "Prefs": "SHORT"
            },
            "productionCertificate": null,
            "sandboxCertificate": {
                "fileName": "sandbox-certificate.p12",
                "data": "<your certificate encoded in base64>",
                "password": "***"
            },
            "gcmApiKey": null,
            "pushGateway": null,
            "developmentApplicationID": null,
            "productionApplicationID": null,
            "sharedSecret": "***",
            "securityIdentifier": null,
            "token": null
        }

### Modify an Application [PUT]

+ Request (application/json)

        {
            "displayName": "Not that new...",
            "name": "com.youandpush.notthatnewanymore",
            "availableLanguages": [
                {
                    "id": "fr" 
                }
            ],
            "sandboxCertificate": {
                "fileName": "another-sandbox-certificate.p12",
                "data": "<your certificate encoded in base64>",
                "password": "your password"
            },
            "discriminatorKeysGroups": {
                "Std": "SHORT",
                "Prefs": "SHORT",
                "AnotherGroup": "LONG"
            }
        }

+ Response 200

# Group Installations

## Installations [/v1/projects/{projectID}/installations]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier

### List Installations [GET]

You can query the following attributes : 
- `advertisingID`  
- `appVersion`  
- `userID`  
- `token`  
- `osVersion`  
- `deviceID`  
- `language`  
- `timeZone`  
- `vendor`  
- `deviceModel`  
- `createdAt`  
- `updatedAt`  
- `lastUsedAt`  
- `status`  
- `statusChangedAt`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=updatedAt,token`

+ Response 200 (application/json)

        [
            {
                "id": "jg19kd8m0rlkqq8k",
                "token": "ee48d4a50e82dd81d8080be510cef35937b784bc50554c528b6a643dd336dc2e",
                "updatedAt": 1398885265000
            },
            {
                "id": "j78kqoq53rkbme34",
                "token": "0619bc2faad94afc07f76668343c62811fa47fe068a0e67ce976c80fb97e6165",
                "updatedAt": 1389023106000
            },
            {
                "id": "nrdrrnrbi2j90dj5",
                "token": "ee48d4a50e82dd81d8080be510cef35937b784bc50554c528b6a643dd336dc2e",
                "updatedAt": 1387559866000
            }
        ]


## Installations [/v1/projects/{projectID}/installations/{installationID}]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    + installationID (required, string, `jad781057n4157r3`) ... Installation unique identifier
    
### Installation Detail [GET]
    
+ Response 200 (application/json)

        {
            "id": "jad781057n4157r3",
            "advertisingID": "8A649AC4-3460-437B-ACE1-4E634075B35C",
            "application": {
                "id": "4c99542973314696"
            },
            "appVersion": "1.0",
            "bundleID": null,
            "createdAt": 1412258765000,
            "deviceModel": "iPhone5,2",
            "language": "fr",
            "lastUsedAt": 1412258765000,
            "libVersion": null,
            "osVersion": "8.0",
            "phoneID": "74607121-AACF-4004-B05D-F6689626EC63",
            "status": "APPLICATION_REMOVED",
            "statusChangedAt": 1412258766000,
            "timeZone": "02:00",
            "token": "10f26ebdd56e3655b8af7258608c57d41cf626529fe1b77e76c1fadf8d58fe10",
            "updatedAt": 1412258766000,
            "userID": "jjayIPados7",
            "vendor": "Apple Inc",
            "discriminatorKeys": {
                "test": [
                    "a1",
                    "a2"
                ]
            }
        }


# Group Security

## Security Keys [/v1/applications/{applicationID}/securityKeys]

List and create Security Keys

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier
    
### List Security Keys [GET]

authentication : **Credentials**

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `apiKey`  
- `sharedSecret`  
- `revoked`  
- `revokedAt`  
- `type`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=updatedAt,apiKey`

+ Response 200 (application/json)

        [
            {
                "id": "c19f3dbb21784fbb",
                "updatedAt": 1410775988928,
                "apiKey: "49441998e38349f1"
            },
            {
                "id": "b0cbb7879575429f",
                "updatedAt": 1410775988837,
                "apiKey: "a7c0f39d7a5544c0"
            }
        ]

### Create Security Key [POST]

authentication : **Credentials**

type can be : **BackEnd** or **Mobile**

+ Request (application/json)

        {
            "type": "BackEnd"
        }

+ Response 201 (application/json)

        {
            "id": "32536579bf01407c"
        }

## Write Security Keys [/v1/applications/{applicationID}/securityKeys/{securityKeyID}]

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier
    + securityKeyID (required, string, `32536579bf01407c`) ... Security Key unique identifier

### Revoke Security Key [DELETE]

+ Response 200


# Group Single Notifications
Notifications back end **You'n Push API**

## Single Notifications [/v1/projects/{projectID}/singleNotifications]

All you need to list and create single notifications

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier

### Create a Single Notification [POST]

The **recipient** node should contain only one attribute that can be : **installationID**, **deviceID** or **userID**

+ Request (application/json)

        {
            "application": {
                "id": "c149bcf842304cc6"
            },
            "recipient": {
                "userID": "stanmarsh"
            },
            "scheduledAt": 1399993192, 
            "expiry": 1440, 
            "tracker": "my tracker", 
            "badge": 1, 
            "message": "salut",
            "soundResource": "news.caf",
            "localizedActionKey": "button_rsrc",
            "localizedMessageKey": "localized_rsrc",
            "localizedMessageArguments": ["south", "park"],
            "notificationType": "TOAST",
            "title": "my title",
            "backTitle": "my back title",
            "launchImage": "news",
            "backgroundImage": "mybackground.png",
            "backBackgroundImage": "mybackbackground.png",
            "contentAvailable": false,
            "collapseKey": "my_collapse_key",
            "customValues": {
                "key1": "value1",
                "key2": "value2"
            }    
        }

+ Response 200


# Group Notification Logs
Notifications back end **You'n Push API**

## Single Notifications [/v1/projects/{projectID}/notificationLogs]

All you need to list notification logs

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier

### List all Notification Logs [GET]

You can query the following attributes :   
- `sentAt`  
- `handledAt`  
- `firstHandlingAt`  
- `massiveNotificationID`  
- `application`   
- `installation`  
- `scheduledAt`  
- `expiry`   
- `tracker`  
- `userID`  
- `phoneID`  
- `badge`  
- `message`  
- `soundResource`  
- `actionLocalizationKey`  
- `singleOKButton`  
- `messageLocalizationKey`  
- `messageLocalizationArguments`  
- `notificationType`  
- `title`  
- `backTitle`  
- `launchImage`  
- `backgroundImage`  
- `backBackgroundImage`  
- `contentAvailable`  
- `collapseKey`  
- `customValues`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=sentAt,message`

+ Response 200 (application/json)

        [
            {
                "id": 118,
                "message": "lalalala",
                "sentAt": 1432648907242,
                "installation": {
                    "id": "iiuv6hba5944hfci"
                }
            },
            {
                "id": 117,
                "message": "yo",
                "sentAt": 1432547077334,
                "installation": {
                    "id": "iiuv6hba5944hfci"
                }
            },
            {
                "id": 116,
                "message": null,
                "sentAt": 1432546520711,
                "installation": {
                    "id": "iiuv6hba5944hfci"
                }
            }
        ]
        

## Single Notifications [/v1/projects/{projectID}/notificationLogs/{notificationLogID}]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    + notificationLogID (required, string, `118`) ... Notification Log unique identifier

### Retreive a Notification Log [GET]

- Response 200 (application/json)

        {
            "id": 118,
            "actionLocalizationKey": null,
            "backBackgroundImage": null,
            "backgroundImage": null,
            "backTitle": null,
            "category": null,
            "collapseKey": null,
            "contentAvailable": false,
            "badge": null,
            "customValues": null,
            "expiry": 1440,
            "firstHandlingAt": 1432648907000,
            "handledAt": 1432648907229,
            "launchImage": null,
            "message": "lalalala",
            "notificationType": null,
            "scheduledAt": null,
            "sentAt": 1432648907242,
            "soundResource": null,
            "title": null,
            "localizedMessageArguments": null,
            "localizedMessageKey": null,
            "localizedTitleArguments": null,
            "localizedTitleKey": null,
            "application": {
                "id": "u67keit4p73uq23o"
            },
            "massiveNotification": null,
            "notificationTracker": null,
            "createdAt": 1432648907000,
            "updatedAt": null,
            "installation": {
                "id": "iiuv6hba5944hfci"
            }
        }

# Group Massive Notifications

## Massive Notifications [/v1/projects/{projectID}/massiveNotifications]

All you need to list and create massive notifications

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    

### Create Massive Notification [POST]

You must set the boolean **draft** to false if you want your notification to be send.
The **scheduledAt** parameter is optional, if not present the notification will be ent immediatly.

+ Request (application/json)

        {
            "draft": false,
            "name": "my awesome massive notification",
            "scheduledAt": 1399993192, 
            "expiry": 1440, 
            "application": {
                "id": "c149bcf842304cc6"
            },
            "tracker": "my tracker",
            "messages": {
                "fr": {
                    "title": "mon titre",
                    "text": "salut"
                },
                "en": {
                    "title": "my title",
                    "text": "hello",
                    "default": true
                }
            },
            "badge": 1, 
            "soundResource": "news.caf",
            "localizedActionKey": "button_rsrc",
            "localizedMessageKey": "localized_rsrc",
            "localizedMessageArguments": "Antoine",
            "notificationType": "TOAST",
            "backTitle": "my back title",
            "launchImage": "news",
            "backgroundImage": "mybackground.png",
            "backBackgroundImage": "mybackbackground.png",
            "contentAvailable": false,
            "category": "MY_CATEGORY",
            "collapseKey": "my_collapse_key",
            "customValues": {
                "key1": "value1",
                "key2": "value2"
            },
            "recipients": {
                "type": "userID",
                "list": ["user1", "user2"]
            },
            "countries": [
                {
                    "id": "FR"
                },
                {
                    "id": "GB"
                }
            ],
            "excludedLanguages": [
                {
                    "id": "fr-Fr"
                },
                {
                    "id": "en-GB"
                }
            ],
            "osVersion": "7.*",
            "applicationVersion": "1.*",
            "daysSinceLastLaunch": {
                "min": 2,
                "max": 10
            },
            "daysSinceInstallation": {
                "min": 2,
                "max": 10
            },
            "discriminatorKeys": {
                "company": ["Apple", "Google"]
            }
        }
        
+ Response 201 (application/json)

        {
            "id": "f483c95d842c47a7"
        }
    
### List All Massive Notifications [GET]

You can query the following attributes :  
- `status`  
- `deletable`  
- `editable`  
- `createdAt`  
- `updatedAt`  
- `handledAt`  
- `firstHandlingAt`  
- `sentAt`  
- `notificationsCounter`  
- `notificationsToSend`  
- `name`  
- `scheduledAt`  
- `expiry`   
- `tracker`  
- `messages`  
- `badge`  
- `soundResource`  
- `localizedActionKey`  
- `localizedMessageKey`  
- `localizedMessageArguments`  
- `notificationType`  
- `backTitle`  
- `launchImage`  
- `backgroundImage`  
- `backBackgroundImage`  
- `contentAvailable`  
- `category`  
- `collapseKey`  
- `customValues`  
- `countries`  
- `excludedLanguages`  
- `osVersion`  
- `applicationVersion`  
- `daysSinceLastLaunch`  
- `daysSinceInstallation`  
- `discriminatorKeys`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=status,deletable,editable`

+ Response 200 (application/json)

        [
            {
                "id": "f483c95d842c47a7",
                "status": "DRAFT",
                "deletable": true,
                "editable": true
            },
            {
                "id": "cc6aefde86ba42b7",
                "status": "SENT",
                "deletable": false,
                "editable": false
            }
        ]

## Massive Notifications [/v1/projects/{projectID}/massiveNotifications/{massiveNotificationID}]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    + massiveNotificationID (required, string, `f483c95d842c47a7`) ... Massive notification unique identifier

### Retrieve Massive Notification [GET]

+ Response 200 (application/json)

        {
            "id": "f483c95d842c47a7",
            "draft": false,
            "status": "SENT",
            "deletable": false,
            "editable": false,
            "createdAt": 1434965338738,
            "updatedAt": 1434965338738,
            "handledAt": 1434965338738,
            "firstHandlingAt": 1434965338738,
            "sentAt": 1434965338738,
            "notificationsCounter": 278394,
            "notificationsToSend": 339645,
            "application": {
                "id": "c149bcf842304cc6"
            },
            "name": "my awesome massive notification",
            "scheduledAt": 1399993192, 
            "expiry": 1440, 
            "tracker": "my tracker",
            "messages": {
                "fr": {
                    "title": "mon titre",
                    "text": "salut",
                    "default": false
                },
                "en": {
                    "title": "my title",
                    "text": "hello",
                    "default": true
                }
            },
            "badge": 1, 
            "soundResource": "news.caf",
            "localizedActionKey": "button_rsrc",
            "localizedMessageKey": "localized_rsrc",
            "localizedMessageArguments": "Antoine",
            "notificationType": "TOAST",
            "backTitle": "my back title",
            "launchImage": "news",
            "backgroundImage": "mybackground.png",
            "backBackgroundImage": "mybackbackground.png",
            "contentAvailable": false,
            "category": "MY_CATEGORY",
            "collapseKey": "my_collapse_key",
            "customValues": {
                "key1": "value1",
                "key2": "value2"
            },
            "countries": [
                {
                    "id": "FR"
                },
                {
                    "id": "GB"
                }
            ],
            "excludedLanguages": [
                {
                    "id": "fr-Fr"
                },
                {
                    "id": "en-GB"
                }
            ],
            "osVersion": "7.*",
            "applicationVersion": "1.*",
            "daysSinceLastLaunch": {
                "min": 2,
                "max": 10
            },
            "daysSinceInstallation": {
                "min": 2,
                "max": 10
            },
            "discriminatorKeys": {
                "company": ["Apple", "Google"]
            }
        }
        
### Modify a Massive Notification [PUT]

Be careful, all the notifications cannot be edited. You have an *editable* attribute that will tell you if you can edit the massive.

+ Request (application/json)

        {
            "name": "My altered super awesome notification",
            "expiry": 600, 
            "tracker": "my other tracker", 
        }

+ Response 200
        
### Remove a Massive Notification [DELETE]

Be careful, all the notifications cannot be deleted. You have a *deletable* attribute that will tell you if you can delete the massive.

+ Response 200

# Group Notification Templates

## Templates [/v1/projects/{projectID}/notificationTemplates]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier

### Create Template [POST]

+ Request (application/json)

        {
            "name": "my awesome massive notification",
            "expiry": 1440,
            "tracker": "my tracker",
            "messages": {
                "fr": {
                    "title": "mon titre",
                    "text": "salut"
                },
                "en": {
                    "title": "my title",
                    "text": "hello",
                    "default": true
                }
            },
            "badge": 1, 
            "soundResource": "news.caf",
            "localizedActionKey": "button_rsrc",
            "localizedMessageKey": "localized_rsrc",
            "localizedMessageArguments": "Antoine",
            "notificationType": "TOAST",
            "backTitle": "my back title",
            "launchImage": "news",
            "backgroundImage": "mybackground.png",
            "backBackgroundImage": "mybackbackground.png",
            "contentAvailable": false,
            "category": "MY_CATEGORY",
            "collapseKey": "my_collapse_key",
            "customValues": {
                "key1": "value1",
                "key2": "value2"
            }
        }
        
+ Response 201 (application/json)

        {
            "id": "0114d6c30e12413f"
        }

### List all Templates [GET]

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `name`  
- `expiry`   
- `tracker`  
- `messages`  
- `badge`  
- `soundResource`  
- `singleOKButton`  
- `localizedActionKey`  
- `localizedMessageKey`  
- `localizedMessageArguments`  
- `notificationType`  
- `title`  
- `backTitle`  
- `launchImage`  
- `backgroundImage`  
- `backBackgroundImage`  
- `contentAvailable`  
- `category`  
- `collapseKey`  
- `customValues`  
  
by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=updatedAt,messages`

+ Response 200 (application/json)

        [
            {
                "id": "0114d6c30e12413f"
            },
            {
                "id": "247974441bbb4f92"
            }
        ]

## Templates [/v1/projects/{projectID}/notificationTemplates/{notificationTemplateID}]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    + notificationTemplateID (required, string, `0114d6c30e12413f`) ... Notification Template unique identifier

### Retrieve a Template [GET]

+ Response 200 (application/json)
    
        {
            "id": "0114d6c30e12413f", 
            "createdAt": 1400061224,
            "updatedAt": 1400061224,
            "name": "my awesome massive notification",
            "expiry": 1440,
            "tracker": "my tracker",
            "messages": {
                "fr": {
                    "title": "mon titre",
                    "text": "salut"
                },
                "en": {
                    "title": "my title",
                    "text": "hello",
                    "default": true
                }
            },
            "badge": 1, 
            "soundResource": "news.caf",
            "localizedActionKey": "button_rsrc",
            "localizedMessageKey": "localized_rsrc",
            "localizedMessageArguments": "Antoine",
            "notificationType": "TOAST",
            "backTitle": "my back title",
            "launchImage": "news",
            "backgroundImage": "mybackground.png",
            "backBackgroundImage": "mybackbackground.png",
            "contentAvailable": false,
            "category": "MY_CATEGORY",
            "collapseKey": "my_collapse_key",
            "customValues": {
                "key1": "value1",
                "key2": "value2"
            }
        }

### Modify a Template [PUT]

+ Request (application/json)

        {
            "name": "Modified Edito Template",
            "expiry": 600, 
            "tracker": "my other tracker", 
        }

+ Response 200

### Delete a Template [DELETE]

+ Response 200

# Group Trackers

## Trackers [/v1/projects/{projectID}/trackers]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier

### List Trackers [GET]

You can query the following attributes :  
- `mobileTracker`  
- `clientTracker`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=mobileTracker,clientTracker`

+ Response 200 (application/json)

        [
            {
                "clientTracker": "tracker1",
                "mobileTracker": "CQ"
            },
            {
                "clientTracker": "tracker2",
                "mobileTracker": "BQ"
            },
            {
                "clientTracker": "tracker3",
                "mobileTracker": "Bg"
            },
            {
                "clientTracker": "tracker4",
                "mobileTracker": "Bw"
            },
            {
                "clientTracker": "tracker5",
                "mobileTracker": "CA"
            }
        ]

# Group Products

Included in the **Purchase** service

## Products [/v1/projects/{projectID}/products]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    

### Create a Product [POST]

+ Request (application/json)

        {
            "name": "My product",
            "comments": "This is my cool product",
            "tags": [
                "aTag",
                "anotherTag"
            ],
            "customValues": {
                "aKey": "aValue";
            }
        }
        
+ Response 201 (application/json)

        {
            "id": "mte78diaq284tesj"
        }
        
        
### List all Products [GET]

authentication : **Credentials**

You can query the following attributes :  
- `createdAt`  
- `updatedAt`  
- `productId`   
- `name`  
- `comments`  
- `publishedAt`  
- `unpublishedAt` 
- `customValues`  
- `parent`  
- `tags`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=createdAt,updatedAt,productId`

+ Response 200 (application/json)

        [
            {
                "id": "mte78diaq284tesj",
                "createdAt": 1424261981000,
                "updatedAt": 1424261981000,
                "productId": "arstarst"
            },
            {
                "id": "b49672cad58c4ef4",
                "createdAt": 1417189018000,
                "updatedAt": 1424261117000,
                "productId": "android.test.purchased"
            },
            {
                "id": "fc546dadd871452c",
                "createdAt": 1417171703000,
                "updatedAt": 1417171703000,
                "productId": "consume"
            },
            {
                "id": "22ac8aacf98d470a",
                "createdAt": 1417105801000,
                "updatedAt": 1417105801000,
                "productId": "premium"
            },
            {
                "id": "d2cc179b87ff4f81",
                "createdAt": 1347298653000,
                "updatedAt": 1424255711000,
                "productId": "9782012384422"
            }
        ]
        

## Products [/v1/projects/{projectID}/products/{productID}]

+ Parameters
    + projectID (required, string, `b23cb94a6fb14e40`) ... Project unique identifier
    + productID (required, string, `mte78diaq284tesj`) ... Product unique identifier

### Product Detail [GET]

+ Response 200 (application/json)

        {
            "id": "mte78diaq284tesj",
            "comments": "This is my cool product",
            "publishedAt": null,
            "tags": [
                "aTag",
                "anotherTag"
            ],
            "name": "My product",
            "unpublishedAt": null,
            "parent": null,
            "createdAt": 1424261981000,
            "updatedAt": 1424261981000,
            "productId": "arstarst",
            "customValues": null
        }
        
### Modify a Product [PUT]

+ Request (application/json)

        {
            "title": "My modified product"
        }
        
+ Response 200

# Group Jobs

## Jobs [/v1/clients/{clientID}/jobs]

+ Parameters
    + clientID (required, string `95e8fc8592184276`) ... Client unique identifier

### Create a Job [POST]

authentication : **Credentials**

+ Request

        {
            "application": {
                "id": "4c99542973314696"
            },
            "classPath": "com.youandpush.myclass",
            "collectionTag": "",
            "cronExpression": "",
            "data": "",
            "disabled": false,
            "idTag": "root",
            "comments": "",
            "parameters": ,
            "notificationTemplate": ,
            "maxDuration": ,
            "maxExecutionTime": ,
            "mustSendEmail": true,
            "name": "CoolJob",
            "url": "www.cooljob.com/coolrssfeed", 
            "withBadge": false,
        }

+ Response 201 (application/json)

        {
            "id": "a99dae833c3a4caa"
        }

### List all Jobs [GET]

authentication : **Credentials**

You can query the following attributes :
- `application`  
- `classPath`  
- `collectionTag`  
- `createdAt`  
- `updatedAt`  
- `cronExpression`  
- `data`  
- `disabled`  
- `firstExecutionAt`  
- `lastExecutionAt`  
- `nextExecutionAt`  
- `idTag`  
- `comments`  
- `parameters`  
- `notificationTemplate`  
- `maxDuration`  
- `maxExecutionTime`  
- `emailFeedback`  
- `name`  
- `url`  
- `withBadge`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.
example : `?fields=name,comments`

+ Response 200 (application/json)

        [
            {
                "id": "a99dae833c3a4caa",
                "name": "CoolJob",
                "comments": "this is a cool job"
            },
            {
                "id": "0717c266732c421b",
                "name": "ShittyJob",
                "comments": "this is a shitty job"
            }
        ]

## Jobs [/v1/clients/{clientID}/jobs/{jobID}]

+ Parameters
    + clientID (required, string `95e8fc8592184276`) ... Client unique identifier
    + jobID (required, string `a99dae833c3a4caa`) ... Job unique identifier

### Job detail [GET]

authentication : **Credentials**

+ Response 200 (application/json)

        {
            "id": "a99dae833c3a4caa",
            "classPath": "fr.sophiacom.ynp.schedularexternalclass.ECExport",
            "collectionTag": null,
            "comments": "this is a cool job",
            "cronExpression": "0 0 21 * * ?",
            "data": null,
            "disabled": true,
            "emailFeedback": false,
            "firstExecutionAt": 1331292481000,
            "idTag": null,
            "lastExecutionAt": 1331809628000,
            "maxDuration": null,
            "maxExecutionTime": null,
            "name": "CoolJob",
            "nextExecutionAt": 1331841600000,
            "parameters": null,
            "url": null,
            "withBadge": false,
            "application": null,
            "notificationTemplate": {
                "id": "0114d6c30e12413f"
            },
            "createdAt": 1331291571000,
            "updatedAt": 1331809791000
        }
        
### Modify a Job [PUT]

+ Request (application/json)

        {
            "name": "my modified Job",
            "comments": "it has been modified"
        }
        
+ Response 200
        
### Delete a Job [DELETE]

authentication : **Credentials**

+ Response 200

# Group Inbox

Included in the **Inbox** service

## Inbox [v1/projects/{projectID}/inbox]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier

### Inbox collection [GET]

authentication: **Credentials**

+ Response 200 (application/json)

        [
            {
                "id": "g09cdlt3d5hn3mqs",
                "userID": "ljenkins"
            },
            {
                "id": "qvcmueoginucr8ss",
                "userID": "npaggle"
            }
        ]

## Inbox [v1/projects/{projectID}/inbox/{inboxID}]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    + inboxID (required, string, `g09cdlt3d5hn3mqs`) ... Inbox unique identifier

### Inbox detail [GET]

+ Response 200 (application/json)

        {
            "id": "g09cdlt3d5hn3mqs",
            "userID": "aberry",
            "project": {
                "id": "aucitbeb99rtteor"
            },
            "messages": [
                {
                    "id": "q9vv9kkn1g1jde0d",
                    "read": false
                },
                {
                    "id": "ouum72l2nu2vek2k",
                    "read": false
                }
            ]
        }
        
# Group Messages

Included in the **Inbox** service
    
## Messages [/v1/projects/{projectID}/messages]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    

### Message Creation [POST]

If the target node is null, the message will be send to everybody

+ Request(application/json)

        {
            "messages": {
                "en": {
                    "title": "targetted ljenkins",
                    "text": "this is a longer text which is the content of the message",
                    "default": true
                },
                "fr": {
                    "title": "ljenkins visé",
                    "text": "je suis le contenu du message, enchanté !"
                }
            },
            "target": {
                "userID": "ljenkins"
            }
        }

+ Response 201 (application/json)

        {
            "id": "q9vv9kkn1g1jde0d"
        }


### Messages Collection [GET]

+ Response 200 (application/json)

        [
            {
                "id": "ouum72l2nu2vek2k"
            },
            {
                "id": "q9vv9kkn1g1jde0d",
                "target": {
                    "userID": "ljenkins"
                }
            }
        ]

## Messages [/v1/projects/{projectID}/messages/{messageID}]

+ Parameters
    + projectID (required, string, `3jjcmr4qbjn41v0g`) ... Project unique identifier
    + messageID (required, string, `q9vv9kkn1g1jde0d`) ... Message unique identifier
    

### Message Detail [GET]

+ Response 200 (application/json)

        {
            "id": "q9vv9kkn1g1jde0d",
            "messages": {
                "en": {
                    "title": "targetted ljenkins",
                    "text": "this is a longer text which is the content of the message",
                    "default": true
                },
                "fr": {
                    "title": "ljenkins visé",
                    "text": "je suis le contenu du message, enchanté !",
                    "default": false
                }
            },
            "target": {
                "userID": "ljenkins"
            }
        }


### Message Update [PUT]

*Be careful*: you cannot change the target

+ Request (application/json)

        {
            "messages": {
                "en": {
                    "title": "I wanted to correct the english message",
                    "defaultMessage": true
                },
                "fr": {
                    "title": "ljenkins visé",
                    "defaultMessage": false
                }
            }
        }

+ Response 200 (application/json)


# Group Statistics

## Statistics Overview Collection [/v1/applications/{applicationID}/statisticsOverview]

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier
    
### Statistics Overview [GET]

You can query the following attributes :  
- `day`  
- `nbAppReleaseCreated`  
- `nbAppReleaseCreatedWithNotificationAccepted`  
- `nbAppReleaseRemoved`  
- `nbLaunchedApplication`  
- `nbMassiveNotification`  
- `nbSingleNotification`  
- `nbTransaction`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=day,nbAppReleaseCreated,nbAppReleaseRemoved,nbLaunchedApplication,nbMassiveNotification,nbSingleNotification,nbTransaction,nbAppReleaseCreatedWithNotificationAccepted`

+ Response 200 (application/json)

        [
            {
                "day": "2014-02-11",
                "nbAppReleaseCreated":2938,
                "nbAppReleaseCreatedWithNotificationAccepted": 2189,
                "nbAppReleaseRemoved":1049,
                "nbLaunchedApplication": 500000,
                "nbMassiveNotification": 1,
                "nbSingleNotification": 60000,
                "nbTransaction": 134
            },
            {
                "day": "2014-02-10",
                "nbAppReleaseCreated":5938,
                "nbAppReleaseCreatedWithNotificationAccepted": 4987,
                "nbAppReleaseRemoved":285,
                "nbLaunchedApplication": 0,
                "nbMassiveNotification": 0,
                "nbSingleNotification": 0,
                "nbTransaction": 0
            },
            {
                "day": "2014-02-09",
                "nbAppReleaseCreated":8294,
                "nbAppReleaseCreatedWithNotificationAccepted": 7392,
                "nbAppReleaseRemoved":2854,
                "nbLaunchedApplication": 0,
                "nbMassiveNotification": 0,
                "nbSingleNotification": 0,
                "nbTransaction": 14923
            }
        ]

## Installation Statistics Collection [/v1/applications/{applicationID}/installationStatistics]

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier
    
### List all Installation Statistics [GET]

You can query the following attributes :  
- `day`  
- `nbAppReleaseCreated`  
- `nbAppReleaseCreatedWithNotificationAccepted`  
- `nbAppReleaseRemoved`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=day,nbAppReleaseCreated,nbAppReleaseRemoved,nbAppReleaseCreatedWithNotificationAccepted`

+ Response 200 (application/json)

        [
            {
                "day": "2014-02-11",
                "nbAppReleaseCreated":2938,
                "nbAppReleaseCreatedWithNotificationAccepted": 2189,
                "nbAppReleaseRemoved":1049
            },
            {
                "day": "2014-02-10",
                "nbAppReleaseCreated":5938,
                "nbAppReleaseCreatedWithNotificationAccepted": 4987,
                "nbAppReleaseRemoved":285
            },
            {
                "day": "2014-02-09",
                "nbAppReleaseCreated":8294,
                "nbAppReleaseCreatedWithNotificationAccepted": 7392,
                "nbAppReleaseRemoved":2854
            }
        ]

## Notification Statistics Collection [/v1/applications/{applicationID}/notificationStatistics]

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier
    
### List all Notification Statistics [GET]

You can query the following attributes :  
- `day`  
- `nbLaunchedApplication`  
- `nbMassiveNotification`  
- `nbSingleNotification`  
- `tracker`

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=day,nbLaunchedApplication,nbMassiveNotification,nbSingleNotification`

+ Response 200 (application/json)

        [
            {
                "day": "2014-02-09",
                "nbLaunchedApplication": 500000,
                "nbMassiveNotification": 1,
                "nbSingleNotification": 60000,
                "tracker": {
                    "clientTracker": "my tracker",
                    "mobileTracker": "AQ"
                }
            },
            {
                "day": "2014-02-09",
                "nbLaunchedApplication": 0,
                "nbMassiveNotification": 0,
                "nbSingleNotification": 0,
                "tracker": {
                    "clientTracker": "my other tracker",
                    "mobileTracker": "AA"
                }
            },
            {
                "day": "2014-02-09",
                "nbLaunchedApplication": 0,
                "nbMassiveNotification": 0,
                "nbSingleNotification": 0,
                "tracker": null
            }
        ]
        

## Purchase Statistics Collection [/v1/applications/{applicationID}/purchaseStatistics]

+ Parameters
    + applicationID (required, string, `4c99542973314696`) ... Application unique identifier

### List all Purchase Statistics [GET]

You can query the following attributes :  
- `day`  
- `nbTransaction`

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=day,nbTransaction`

+ Response 200 (application/json)

        [
            {
                "day": "2014-02-11",
                "nbTransaction": 134
            },
            {
                "day": "2014-02-10",
                "nbTransaction": 0
            },
            {
                "day": "2014-02-09",
                "nbTransaction": 14923
            }
        ]

## Monthly Statistics Collection [/v1/clients/{clientID}/monthlyStatistics]

+ Parameters
    + clientID (required, string, `95e8fc8592184276`) ... Client unique identifier
    
### List all Monthly Statistics [GET]

You can query the following attributes :  
- `nbNotification`  
- `nbTransaction`  
- `yearMonth`  

by adding at the end of the route all the fields you want to retrieve separated by a comma.  
example : `?fields=yearMonth,nbNotification,nbTransaction`

+ Response 200 (application/json)

        [
            {
                "yearMonth": "2014-02",
                "nbNotification": null,
                "nbTransaction": 0
            },
            {
                "yearMonth": "2014-01", 
                "nbNotification": 2,
                "nbTransaction": 0
            },
            {
                "yearMonth": "2013-12",
                "nbNotification": 7,
                "nbTransaction": 0
            }
        ]
