# Endpoints API

## General
The Endpoints API allows creating, updating, reading and deleting of endpoints  

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens
In addition, using the Kibana API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***List Endpoints***
----
  Return list of configured account endpoints.

* **URL**

  https://api.logz.io/v1/endpoints

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
[
  {
    "endpointType": "Slack",
    "id": 1,
    "title": "example",
    "description": "this is an example",
    "url": "http://www.example.com"
  },
  {
    "endpointType": "BigPanda",
    "id": 2,
    "title": "second-example",
    "description": "This is another example",
    "apiToken": "ccc0d7a2ce50b365ffca653e80d52ceb",
    "appKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
  }
]
```

* **Swagger Specification**
```json
{
    "/endpoints": {
      "get": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Return list of configured account endpoints.",
        "description": "",
        "operationId": "list",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "type": "array",
              "items": {
                "$ref": "#/definitions/Endpoint"
              }
            }
          }
        }
      }
    },
     "Endpoint": {
       "type": "object",
       "properties": {
         "endpointType": {
           "type": "string",
           "enum": [
             "BigPanda",
             "Slack",
             "Datadog",
             "Custom",
             "PagerDuty",
             "VictorOps"
           ]
         },
         "id": {
           "type": "integer",
           "format": "int32"
         },
         "title": {
           "type": "string"
         },
         "description": {
           "type": "string"
         }
       }
     }
}
```

***Get Endpoint***
----
  Return endpoint by id.

* **URL**

  https://api.logz.io/v1/endpoints/{id}

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint Id|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
    "endpointType": "Slack",
    "id": 1,
    "title": "example",
    "description": "this is an example",
    "url": "http://www.example.com"
}
```

* **Swagger Specification**
```json
{
    "/endpoints/{id}": {
      "get": {
        "summary": "Return endpoint by id.",
        "description": "",
        "operationId": "get",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/Endpoint"
            }
          }
        }
      }
    },
    "Endpoint": {
        "type": "object",
        "properties": {
          "endpointType": {
            "type": "string",
            "enum": [
              "BigPanda",
              "Slack",
              "Datadog",
              "Custom",
              "PagerDuty",
              "VictorOps"
            ]
          },
          "id": {
            "type": "integer",
            "format": "int32"
          },
          "title": {
            "type": "string"
          },
          "description": {
            "type": "string"
          }
        }
    }
}
```

***Delete Endpoint***
----
  Delete an endpoint by id.

* **URL**

  https://api.logz.io/v1/endpoints/{id}

* **HTTP Method:**

  `DELETE`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint Id|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
```

* **Swagger Specification**
```json
{
    "/endpoints/{id}": {
        "delete": {
            "tags": [
              "Endpoints"
            ],
            "summary": "Delete endpoint by id.",
            "description": "",
            "operationId": "delete",
            "consumes": [
              "application/json"
            ],
            "produces": [
              "application/json"
            ],
            "parameters": [
              {
                "name": "x-forwarded-for",
                "in": "header",
                "required": false,
                "type": "string"
              },
              {
                "name": "id",
                "in": "path",
                "required": true,
                "type": "integer",
                "format": "int32"
              },
              {
                "name": "X-API-TOKEN",
                "in": "header",
                "description": "Authentication Api Token",
                "required": false,
                "type": "string"
              }
            ],
            "responses": {
              "default": {
                "description": "successful operation"
              }
            }
        }
    }
}
```

***Create Slack Endpoint***
----
  Create Slack endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/slack

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| url| string| Slack URL|
---

* **Example**
```json
{
  "title": "slack-example",
  "description": "this is an example",
  "url": "http://www.example.com"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 3
}
```

* **Swagger Specification**
```json
{
 "/endpoints/slack": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create Slack endpoint",
        "description": "",
        "operationId": "createSlack",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/SlackEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "SlackEndpointUpsertRequest": {
        "type": "object",
        "properties": {
         "title": {
           "type": "string"
         },
         "description": {
           "type": "string"
         },
         "url": {
           "type": "string"
         }
        }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update Slack Endpoint***
----
  Update Slack endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/slack/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint ID|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| url| string| Slack URL|
---

* **Example**
```json
{
  "title": "slack-example",
  "description": "this is an example",
  "url": "http://www.example.com"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 3
}
```

* **Swagger Specification**
```json
{
"/endpoints/slack/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update Slack endpoint",
        "description": "",
        "operationId": "updateSlack",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/SlackEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "SlackEndpointUpsertRequest": {
        "type": "object",
        "properties": {
         "title": {
           "type": "string"
         },
         "description": {
           "type": "string"
         },
         "url": {
           "type": "string"
         }
        }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Create PagerDuty Endpoint***
----
  Create PagerDuty endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/pager-duty

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint Name| 
| description| string| Endpoint description|
| serviceKey| string| PagerDuty service key|
---

* **Example**
```json
{
  "title": "pager-duty-example",
  "description": "this is an example",
  "serviceKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 3
}
```

* **Swagger Specification**
```json
{
 "/endpoints/pager-duty": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create PagerDuty endpoint",
        "description": "",
        "operationId": "createPagerDuty",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/PagerDutyEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "PagerDutyEndpointUpsertRequest": {
        "type": "object",
        "properties": {
         "title": {
           "type": "string"
         },
         "description": {
           "type": "string"
         },
         "serviceKey": {
           "type": "string"
         }
        }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update PagerDuty Endpoint***
----
  Update PagerDuty endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/pager-duty/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint Id|
| title| string| Endpoint name|  
| description| string| Endpoint description|
| serviceKey| string| PagerDuty service key|
---

* **Example**
```json
{
  "title": "pager-duty-example",
  "description": "this is an example",
  "serviceKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 4
}
```

* **Swagger Specification**
```json
{
    "/endpoints/pager-duty/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update PagerDuty endpoint",
        "description": "",
        "operationId": "updatePagerDuty",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/PagerDutyEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },    
    "PagerDutyEndpointUpsertRequest": {
        "type": "object",
        "properties": {
         "title": {
           "type": "string"
         },
         "description": {
           "type": "string"
         },
         "serviceKey": {
           "type": "string"
         }
        }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Create BigPanda Endpoint***
----
  Create BigPanda endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/big-panda

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| appKey| string| BigPanda app key|
| apiToken| string| BigPanda api token|
---

* **Example**
```json
{
  "title": "big-panda-example",
  "description": "this is an example",
  "apiToken": "ccc0d7a2ce50b365ffca653e80d52ceb",
  "appKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 4
}
```

* **Swagger Specification**
```json
{
    "/endpoints/big-panda": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create BigPanda endpoint",
        "description": "",
        "operationId": "createBigPanda",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/BigPandaEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "BigPandaEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "apiToken": {
          "type": "string"
        },
        "appKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update BigPanda Endpoint***
----
  Update BigPanda endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/big-panda/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint Id|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| appKey| string| BigPanda app key|
| apiToken| string| BigPanda api token|
---

* **Example**
```json
{
  "title": "big-panda-example",
  "description": "this is an example",
  "apiToken": "ccc0d7a2ce50b365ffca653e80d52ceb",
  "appKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 5
}
```

* **Swagger Specification**
```json
{
    "/endpoints/big-panda/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update BigPanda endpoint",
        "description": "",
        "operationId": "updateBigPanda",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/BigPandaEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },    
    "BigPandaEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "apiToken": {
          "type": "string"
        },
        "appKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Create DataDog Endpoint***
----
  Create DataDog endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/data-dog

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| apiKey| string| DataDog api key|
---

* **Example**
```json
{
  "title": "datadog-example",
  "description": "this is an example",
  "apiKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"

}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 4
}
```

* **Swagger Specification**
```json
{
    "/endpoints/data-dog": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create Datadog endpoint",
        "description": "",
        "operationId": "createDataDog",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/DatadogEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "DatadogEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "apiKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update DataDog Endpoint***
----
  Update DataDog endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/data-dog/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint Id|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| apiKey| string| Endpoint api key|
---

* **Example**
```json
{
  "title": "datadog-example",
  "description": "this is an example",
  "apiKey": "72ea2ed8ae331fbc80c7bdaffabc8f76"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 5
}
```

* **Swagger Specification**
```json
{
    "/endpoints/data-dog/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update Datadog endpoint",
        "description": "",
        "operationId": "updateDataDog",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/DatadogEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "DatadogEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "apiKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Create VictorOps Endpoint***
----
  Create VictorOps endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/victorops

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| serviceApiKey| string| VictorOps service api key|
| routingKey| string| VictorOps routing key|
| messageType| string| VictorOps message type|
---

* **Example**
```json
{
  "title":"victor-ops-example",
  "description":"This is an example",
  "serviceApiKey":"12345678-1234-1234-1234-1234567890",
  "messageType":"CRITICAL",
  "routingKey":"route-key"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 4
}
```

* **Swagger Specification**
```json
{
    "/endpoints/victorops": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create VictorOps endpoint",
        "description": "",
        "operationId": "createVictorops",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/VictoropsEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "VictoropsEndpointUpsertRequest": {
      "type": "object",
      "required": [
        "messageType",
        "routingKey",
        "serviceApiKey"
      ],
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "routingKey": {
          "type": "string"
        },
        "messageType": {
          "type": "string"
        },
        "serviceApiKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update VictorOps Endpoint***
----
  Update VictorOps endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/victorops/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint id| 
| title| string| Endpoint name| 
| description| string| Endpint description|
| serviceApiKey| string| VictorOps service api key|
| routingKey| string| VictorOps routing key|
| messageType| string| VictorOps message type|
---

* **Example**
```json
{
  "id":5,
  "title":"victor-ops-example",
  "description":"This is an example",
  "serviceApiKey":"12345678-1234-1234-1234-1234567890",
  "messageType":"CRITICAL",
  "routingKey":"route-key"
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 5
}
```

* **Swagger Specification**
```json
{
    "/endpoints/victorops/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update VictorOps endpoint",
        "description": "",
        "operationId": "updateVictorops",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/VictoropsEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "VictoropsEndpointUpsertRequest": {
      "type": "object",
      "required": [
        "messageType",
        "routingKey",
        "serviceApiKey"
      ],
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "routingKey": {
          "type": "string"
        },
        "messageType": {
          "type": "string"
        },
        "serviceApiKey": {
          "type": "string"
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Create Custom Endpoint***
----
  Create custom endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/custom

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| title| string| Endpoint name| 
| description| string| Endpoint description|
| url| string| Endpoint URL|
| method| string| POST/GET/PUT| 
| headers| string|Request headers should be separated by comma|
| bodyTemplate| string| Body Template JSON|
---

* **Example**
```json
{
    "endpointType": "Custom",
    "id": 4,
    "title": "custom-example-endpoint",
    "description": "This is an example endpoint",
    "url": "http://www.custom.com",
    "method": "POST",
    "headers": "header=header-value",
    "bodyTemplate": {
      "alert_title": "{{alert_title}}"
    }
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 4
}
```

* **Swagger Specification**
```json
{
    "/endpoints/custom": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Create custom endpoint",
        "description": "",
        "operationId": "createCustom",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/CustomEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "CustomEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "url": {
          "type": "string"
        },
        "method": {
          "type": "string"
        },
        "headers": {
          "type": "string"
        },
        "bodyTemplate": {
          "type": "object",
          "additionalProperties": {
            "type": "object"
          }
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

***Update Custom Endpoint***
----
  Update custom endpoint.

* **URL**

  https://api.logz.io/v1/endpoints/custom/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| Endpoint id| 
| title| string| Endpoint name|   
| description| string| Endpoint description|
| url| string| Endpoint URL|
| method| string| POST/GET/PUT|
| headers| string|Request headers should be separated by comma|
| bodyTemplate| string| Body template JSON|
---

* **Example**
```json
{
    "endpointType": "Custom",
    "id": 4,
    "title": "custom-example-endpoint",
    "description": "This is an example endpoint",
    "url": "http://www.custom.com",
    "method": "POST",
    "headers": "header=header-value",
    "bodyTemplate": {
      "alert_title": "{{alert_title}}"
    }
}
```
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "id": 5
}
```

* **Swagger Specification**
```json
{
    "/endpoints/custom/{id}": {
      "put": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Update custom endpoint",
        "description": "",
        "operationId": "updateCustom",
        "consumes": [
          "application/json"
        ],
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "x-forwarded-for",
            "in": "header",
            "required": false,
            "type": "string"
          },
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          },
          {
            "in": "body",
            "name": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/CustomEndpointUpsertRequest"
            }
          },
          {
            "name": "X-API-TOKEN",
            "in": "header",
            "description": "Authentication Api Token",
            "required": false,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "successful operation",
            "schema": {
              "$ref": "#/definitions/EndpointUpsertResponse"
            }
          }
        }
      }
    },
    "CustomEndpointUpsertRequest": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string"
        },
        "description": {
          "type": "string"
        },
        "url": {
          "type": "string"
        },
        "method": {
          "type": "string"
        },
        "headers": {
          "type": "string"
        },
        "bodyTemplate": {
          "type": "object",
          "additionalProperties": {
            "type": "object"
          }
        }
      }
    },
    "EndpointUpsertResponse": {
        "type": "object",
        "properties": {
         "id": {
           "type": "integer",
           "format": "int32"
         }
        }
    }
}
```

### API specification in Swagger format
[JSON file here](swagger.json)
