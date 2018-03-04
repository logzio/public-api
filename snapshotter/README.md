# Snapshots API

## General
The Snapshots API allows creating and sending snapshots to endpoints and email recipients 

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens
In addition, using the Snapshotter API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***Create Snapshot***
----
  Create snapshot

* **URL**

  https://api.logz.io/v1/snapshotter

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| snapshotType| enum| DASHBOARD/VISUALIZATION| 
| snapshotSavedObjectId| string| Id of the kibana saved object (can be retrieve by using kibana/export API)|
| endpoints| array| Array of endpoint ids that will recieve the snapshot. Only Slack endpoint is supported|
| emails| array| Array of email recipients that will recieve the snapshot|
| message| string| Message attached to the snapshot|
| timeFrameFrom| string| Start time of the snapshot|
| timeFrameTo| string| End time of the snapshot|
| snapshotTimeZone| string| TimeZone of the snapshot|
| queryString| string| Query search input|
| darkTheme| boolean| true = dark|
---

* **Example**
```json
{
  "snapshotType": "VISUALIZATION",
  "snapshotSavedObjectId": "example-visualization",
  "endpoints": [
    3
  ],
  "emails": [
    "example-mail@logz.io"
  ],
  "message": "This is an example message",
  "timeFrameFrom": "2018-02-08T09:28:01.187Z",
  "timeFrameTo": "2018-02-08T09:43:01.188Z",
  "snapshotTimeZone": "Asia/Jerusalem",
  "queryString": "type:example",
  "darkTheme": true
}
```
* **Success Response:**

**Code:** 200 OK

---
| Parameter|Type|Description|
|---|---|---|
| snapshotId| int| | 
---  
**Content:** 
```json
{
  "snapshotId": 2
}
```

* **Swagger Specification**
```json
{
   "paths": {
        "/snapshotter": {
          "post": {
            "tags": [
              "Snapshotter"
            ],
            "summary": "Create snapshot",
            "description": "",
            "operationId": "createSnapshot",
            "consumes": [
              "application/json"
            ],
            "produces": [
              "application/json"
            ],
            "parameters": [
              {
                "in": "body",
                "name": "body",
                "required": false,
                "schema": {
                  "$ref": "#/definitions/SnapshotCreateRequest"
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
                  "$ref": "#/definitions/SnapshotCreateResponse"
                }
              }
            }
          }
        }
    },
    "definitions": {
       "SnapshotCreateResponse": {
         "type": "object",
         "properties": {
           "snapshotId": {
             "type": "integer",
             "format": "int32"
           }
         }
       },
            "SnapshotCreateRequest": {
          "type": "object",
          "required": [
            "snapshotTimeZone",
            "snapshotType",
            "timeFrameFrom",
            "timeFrameTo"
          ],
          "properties": {
            "snapshotType": {
              "type": "string",
              "enum": [
                "DASHBOARD",
                "VISUALIZATION",
                "SAVED_SEARCH"
              ]
            },
            "snapshotSavedObjectId": {
              "type": "string"
            },
            "endpoints": {
              "type": "array",
              "items": {
                "type": "integer",
                "format": "int32"
              }
            },
            "emails": {
              "type": "array",
              "items": {
                "type": "string"
              }
            },
            "message": {
              "type": "string"
            },
            "timeFrameFrom": {
              "type": "integer",
              "format": "int64"
            },
            "timeFrameTo": {
              "type": "integer",
              "format": "int64"
            },
            "snapshotTimeZone": {
              "type": "string"
            },
            "queryString" : {
              "type" : "string"
            },
            "darkTheme": {
              "type": "boolean"
            }
          }
        }
    }
}
```

***Get Snapshot***
----
  Return snapshot by id.

* **URL**

  https://api.logz.io/v1/snapshotter/{id}

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| snapshotId| int| Snapshot Id|
---

* **Success Response:**

**Code:** 200 OK
---
| Parameter|Type|Description|
|---|---|---|
| snapshotId| int| snapshot Id|
| status| enum| "SUCCESS", "FAILED", "IN_PROGRESS"|
| snapshotSavedObjectId| string| Name of the kibana saved object|
| imageUrl| string|Snapshot image file URL |
| appLinkUrl| string|Snapshot URL in the logz.io application|
| message| string| Message attached to the snapshot|
| timeFrameFrom| string| Start time of the snapshot in epoch format|
| timeFrameTo| string| End time of the snapshot in epoch format|
| snapshotTimeZone| string| TimeZone of the snapshot|
---
  
**Content:** 
```json
{
  "snapshotId": 2,
  "status": "SUCCESS",
  "snapshotSavedObjectId": "example-visualization",
  "imageUrl": "http://6f1a0d6a6baf:9000/#/dashboard/kibana?embed=true&snapshot_mode=true&&force_snapshot_timezone=Asia%2FJerusalem&_g=%28time%3A%28from%3A%272018-02-08T09%3A28%3A01.187Z%27%2Cmode%3Aabsoloute%2Cto%3A%272018-02-08T09%3A43%3A01.188Z%27%29%29&kibanaRoute=%2Fvisualize%2Fedit%2Fexample-visualization&theme=dark&shareToken=02b4d55f-1795-45b2-aa02-530ed54a27ac",
  "appLinkUrl": "http://6f1a0d6a6baf:9000/#/dashboard/kibana?&_g=%28time%3A%28from%3A%272018-02-08T09%3A28%3A01.187Z%27%2Cmode%3Aabsoloute%2Cto%3A%272018-02-08T09%3A43%3A01.188Z%27%29%29&kibanaRoute=%2Fvisualize%2Fedit%2Fexample-visualization&theme=dark?switchToAccountId=3",
  "message": "This is an example message",
  "timeFrameFrom": 1518082081,
  "timeFrameTo": 1518082981,
  "snapshotTimeZone": "Asia/Jerusalem"
}
```

* **Swagger Specification**
```json
{
  "paths": {
      "/snapshotter/{snapshotId}": {
        "get": {
          "tags": [
            "Snapshotter"
          ],
          "summary": "Get snapshot detail by id",
          "description": "",
          "operationId": "createSnapshot",
          "consumes": [
            "application/json"
          ],
          "produces": [
            "application/json"
          ],
          "parameters": [
            {
              "name": "snapshotId",
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
                "$ref": "#/definitions/SnapshotGetResponse"
              }
            }
          }
        }
      }
  },
  "definitions": {
    "SnapshotGetResponse": {
          "type": "object",
          "properties": {
            "snapshotId": {
              "type": "integer",
              "format": "int32"
            },
            "status": {
              "type": "string",
              "enum": [
                "SUCCESS",
                "FAILED",
                "IN_PROGRESS"
              ]
            },
            "snapshotSavedObjectId": {
              "type": "string"
            },
            "imageUrl": {
              "type": "string"
            },
            "appLinkUrl": {
              "type": "string"
            },
            "message": {
              "type": "string"
            },
            "timeFrameFrom": {
              "type": "integer",
              "format": "int64"
            },
            "timeFrameTo": {
              "type": "integer",
              "format": "int64"
            },
            "snapshotTimeZone": {
              "type": "string"
            }
          }
        }
    }
}
```

### API specification in Swagger format
[JSON file here](swagger.json)