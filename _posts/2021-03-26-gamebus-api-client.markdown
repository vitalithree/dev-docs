---
layout: post
title:  "GameBus API Client"
date:   2021-03-26 10:06:51 +0200
---

## How can I implement my own client?

### Basic GET Implementation

**Unirest-Java** can be a good choice here, as it is a very easy to use lightweight HTTP client library. An example of a basic implementation made on top of it able to send a GET request to GameBus api is as follows:

```java
/**
 * GET request able to receive an object as response.
 *
 * @param action to be requested
 * @param queryParams used by the request
 * @param responseClass for generic types inference
 * @param onSuccess callback for success handling
 * @param onFailure callback for error handling
 * @param <T> response object type
 * @return The http response as an object
 */
public static <T> T GET(String action, Map<String, Object> queryParams, Class<T> responseClass, Consumer<HttpResponse<T>> onSuccess, Consumer<HttpResponse<Error>> onFailure) {
	return Unirest.get(String.format("%s/%s", Config.API.getUrl(), action))
                .queryString(queryParams)
                .asObject(responseClass)
                .ifSuccess(onSuccess)
                .ifFailure(Error.class, onFailure)
                .getBody();
}
```

where

```java
Config.API.getUrl()
```

points to the GameBus api url in use (e.g. `api3.gamebus.eu`), and is set by an specialized configuration object holding all needed constants/values.

It is worth noting that this implementation is here only to illustrate how easy it is to extend/make use of **Unirest-Java** to implement your own client. But it is worth mentioning as well that any implementation able to use the GameBus API is fine, as long as it does the trick for the developer.

### Error Handling

It's important to highlight the two callback parameters that can be used for error handling:

```java
Consumer<HttpResponse<T>> onSuccess
```

is called when the requests succeeds, so the client is able to make use of the information stored in the HTTP response object passed as argument to the callback.

```java
Consumer<HttpResponse<Error>> onFailure
```

is triggered on failure, and the HTTP response object holds information about the issue/error that happened causing the request/response failure.

For more information about how it works: <https://kong.github.io/unirest-java/#responses>

### Request Header for Authorization

The authorization token that allows the requests to be performed is mandatory, as mentioned before, and must be always set as a header. By using **Unirest-Java**'s config object this can be set as a default Header for every request:

```java
Unirest.config().setDefaultHeader("Authorization", String.format("Bearer %s", Config.AUTH.getToken()));
```

where

```java
Config.AUTH.getToken()
```

contains the client's authorization token value.

#### Creating Pojo classes from response JSON
You can create Pojo classes from the json responses online using sites like: <http://www.jsonschema2pojo.org/>
There is also a possibility to add a gradle or maven plugin which can generate the classes for you as well.

## Game Descriptors

In order to have an overview on all available GameDescriptors, please have a look at the json response from ActivitySchemes GET request.

### Request structure & Example response

<https://api.gamebus.eu/v2/swagger-ui/index.html#/activities/activitiesSchemesGet>

## User SignUp
<https://api.gamebus.eu/v2/swagger-ui/index.html#/users/signUp>

## Create and store an activity using names and translation keys (application/json content type)

### Request structure
```
$ curl 'http://localhost:8080/me/activities' -i -X POST \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 455a2d79-d83a-4445-aa8b-64fd7cf4ed50' \
    -d '{
  "gameDescriptorTK" : "WALK",
  "dataProviderName" : "Google Fit",
  "image" : "http://kowedes.nl/gb/mock/activities/run-together.jpg",
  "date" : 1614663499477,
  "propertyInstances" : [ {
    "propertyTK" : "STEPS",
    "value" : 200
  } ],
  "players" : [ 103766 ]
}'
```

### Example response

```
HTTP/1.1 200 OK
```

## Create and store an activity using IDs (application/json content type)

### Request structure

```
$ curl 'http://localhost:8080/me/activities' -i -X POST \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 000000ijjk876-d83a-4445-aa8b-000000jsjaj77' \
    -d '{
  "gameDescriptor" : 68,
  "dataProvider" : 1,
  "miniGameUse" : 1,
  "date" : 1614663499477,
  "propertyInstances" : [ {
    "property" : 112,
    "value" : "{\"type\":\"word\",\"word\":{\"x\":9,\"y\":0,\"d\":\"y\",\"word\":\"emir\",\"input\":\"****\",\"question\":\"Arabische titel\",\"complete\":true},\"timestamp\":1614663505305}"
  }, {
    "property" : 113,
    "value" : "{\"type\":\"word\",\"word\":{\"x\":9,\"y\":0,\"d\":\"y\",\"word\":\"emir\",\"input\":\"****\",\"question\":\"Arabische titel\",\"complete\":true},\"timestamp\":1614663505305}"
  }, {
    "property" : 114,
    "value" : true
  }, {
    "property" : 115,
    "value" : null
  }, {
    "property" : 116,
    "value" : false
  }, {
    "property" : 117,
    "value" : null
  }, {
    "property" : 118,
    "value" : "1"
  }, {
    "property" : 119
  } ],
  "players" : [ 495 ]
}'
```

### Example response

```
HTTP/1.1 200 OK
```

## Create and store an activity (multipart/form-data content type)

### Request structure

```
$ curl --location --request POST 'http://localhost:8024/v2/activities?dryrun=false' \
--header 'Content-Type: multipart/form-data' \
--header 'Authorization: Bearer 4285eeeekk61-ca78-461d-9f45-hsgah7777829' \
--form 'activity="{
   \"gameDescriptor\":1,
   \"dataProvider\":5,
   \"image\": \"http://kowedes.nl/gb/mock/activities/run-together.jpg\",
   \"date\": 1616422246211,
   \"propertyInstances\":[
      {
         \"property\": 1,
         \"value\":15
      }
   ],
   \"players\":[
       459
   ]
}"'
```

### Example response

```
HTTP/1.1 201 Created
Content-Type: application/json;charset=UTF-8


[
    {
        "id": 20024,
        "date": 1605795049041,
        "isManual": false,
        "group": "MTYwNTc4MDIyODk0M2VmaGZIeEFa",
        "image": "http://kowedes.nl/gb/mock/activities/run-together.jpg",
        "creator": {
            "id": 473,
            "user": {
                "id": 465,
                "firstName": "Jenkins",
                "lastName": "User",
                "image": null
            }
        },
        "player": {
            "id": 473,
            "user": {
                "id": 465,
                "firstName": "Jenkins",
                "lastName": "User",
                "image": null
            }
        },
        "gameDescriptor": {
            "id": 2,
            "translationKey": "RUN",
            "image": "https://api3.gamebus.eu/v2/uploads/public/brand/gd/icon/RUN.png",
            "type": "PHYSICAL",
            "isAggregate": false
        },
        "dataProvider": {
            "id": 5,
            "name": "Google Fit",
            "image": "https://api3.gamebus.eu/v2/uploads/public/brand/dp/GoogleFit.png",
            "isConnected": false
        },
        "propertyInstances": [
            {
                "id": 51344,
                "value": "654",
                "property": {
                    "id": 1,
                    "translationKey": "STEPS",
                    "baseUnit": "count",
                    "inputType": "INT",
                    "aggregationStrategy": "SUM",
                    "propertyPermissions": [
                        {
                            "id": 646,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 650,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 653,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 658,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 663,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 668,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 674,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 678,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 681,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 686,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 691,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 696,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 729,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        },
                        {
                            "id": 740,
                            "index": 0,
                            "lastUpdate": null,
                            "decisionNote": null,
                            "state": "PUBLIC_APPROVED",
                            "allowedValues": []
                        }
                    ]
                }
            }
        ],
        "personalPoints": [],
        "supports": null,
        "chats": null
    }
]
```

## Delete an already existing activity
<https://api.gamebus.eu/v2/swagger-ui/index.html#/activities/activityDelete>

## Read an already existing activity
<https://api.gamebus.eu/v2/swagger-ui/index.html#/activities/activityGet>

### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: 3408

{
  "id" : 20006,
  "date" : 1605622222003,
  "isManual" : true,
  "group" : "MTYwNTYyNDc0MjM4NExEbmlvdHdu",
  "image" : "http://kowedes.nl/gb/mock/activities/run-together.jpg",
  "creator" : {
    "id" : 473,
    "user" : {
      "id" : 465,
      "firstName" : "Jenkins",
      "lastName" : "User",
      "image" : null
    }
  },
  "player" : {
    "id" : 473,
    "user" : {
      "id" : 465,
      "firstName" : "Jenkins",
      "lastName" : "User",
      "image" : null
    }
  },
  "gameDescriptor" : {
    "id" : 2,
    "translationKey" : "RUN",
    "image" : "https://api3.gamebus.eu/v2/uploads/public/brand/gd/icon/RUN.png",
    "type" : "PHYSICAL",
    "isAggregate" : false
  },
  "dataProvider" : {
    "id" : 1,
    "name" : "GameBus",
    "image" : "https://api3.gamebus.eu/v2/uploads/public/brand/dp/GameBus.png",
    "isConnected" : false
  },
  "propertyInstances" : [ {
    "id" : 51316,
    "value" : "500",
    "property" : {
      "id" : 1,
      "translationKey" : "STEPS",
      "baseUnit" : "count",
      "inputType" : "INT",
      "aggregationStrategy" : "SUM",
      "propertyPermissions" : [ {
        "id" : 646,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 650,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 653,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 658,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 663,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 668,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 674,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 678,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 681,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 686,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 691,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 696,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 729,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      }, {
        "id" : 740,
        "index" : 0,
        "lastUpdate" : null,
        "decisionNote" : null,
        "state" : "PUBLIC_APPROVED"
      } ]
    }
  } ],
  "personalPoints" : [ ],
  "supports" : null,
  "chats" : null
}
```
