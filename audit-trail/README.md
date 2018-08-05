# Audit-Trail API

## General
The Audit Trail API allows querying audit events  

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/api-tokens
In addition, using the Kibana API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/api-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***List Account Audit trail events***
----
  Returns a json list of audit events of an account.

* **URL**

  https://api.logz.io/v1/audit-trail

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| auditEventType| Object|Event type to filter. Defined as one of the elements in the results retrieved by "Event Type" API|
| auditEventUser| Object|User to filter|
| fromDate| integer| Start time of the audit list |
| toDate| integer| End time of the audit list |
| includeFiltersData| boolean| Should include the audit list as part of the result|
| size| integer|number of rows to return|
| sortDescending|boolean| Sorting order. True=Descending|
---

* **Swagger Specification**
```json
{
    "/audit-trail": {
        "post": {
            "consumes": [
                "application/json"
            ],
            "produces": [
                "application/json"
            ],
            "parameters": [
                {
                    "name": "body",
                    "in": "body",
                    "required": true,
                    "schema": {
                        "$ref": "#/definitions/AuditTrailFilterRequest"
                    }
                }
            ],
            "responses": {
                "200": {
                    "description": "OK",
                    "headers": {
                    },
                    "schema": {
                        "$ref": "#/definitions/AuditTrailFilteredResponse"
                    }
                }
            }
        }
    },
   "AuditTrailFilterRequest": {
        "properties": {
            "auditEventType": {
                "type": "string"
            },
            "auditEventUser": {
                "$ref": "#/definitions/AuditEventUser"
            },
            "from": {
                "type": "integer"
            },
            "fromDate": {
                "type": "integer"
            },
            "includeFiltersData": {
                "type": "boolean"
            },
            "size": {
                "type": "integer"
            },
            "sortDescending": {
                "type": "boolean"
            },
            "toDate": {
                "type": "integer"
            }
        }
    },
    "AuditTrailFilteredResponse": {
        "properties": {
            "auditEventTypesList": {
                "type": "array",
                "items": {
                    "$ref": "#/definitions/AuditEventTypeData"
                }
            },
            "auditEventUsersList": {
                "type": "array",
                "items": {
                    "$ref": "#/definitions/AuditEventUser"
                }
            },
            "from": {
                "type": "integer"
            },
            "pageSize": {
                "type": "integer"
            },
            "results": {
                "type": "array",
                "items": {
                    "$ref": "#/definitions/Object"
                }
            },
            "total": {
                "type": "integer"
            }
        }
    },
    "AuditEventTypeData": {
         "properties": {
             "auditEventType": {
                 "type": "string"
             },
             "auditEventTypeTitle": {
                 "type": "string"
             }
         }
     },
     "AuditEventUser": {
         "properties": {
             "deleted": {
                 "type": "boolean"
             },
             "fullName": {
                 "type": "string"
             },
             "id": {
                 "type": "integer"
             },
             "userToken": {
                 "type": "boolean"
             }
         }
     }
}
```
***example***
```json
{
  "pageSize": 15,
  "from": 0,
  "total": 1,
  "results": [
    {
      "auditEventUser": {
        "id": 1,
        "fullName": "testadmin@logz.io",
        "userToken": false,
        "deleted": false
      },
      "date": 1516538068641,
      "auditEventTypeTitle": "Added user",
      "ip": null,
      "geoLocation": null,
      "extraDataList": [
        {
          "fieldName": "Name",
          "oldValue": null,
          "newValue": "a@a.com"
        }
      ],
      "valid": true
    }
  ],
  "auditEventUsersList": [
    {
      "id": 1,
      "fullName": "testadmin@logz.io",
      "userToken": false,
      "deleted": false
    }
  ],
  "auditEventTypesList": [
    {
      "auditEventType": "ADD_USER",
      "auditEventTypeTitle": "Added user"
    }
  ]
}
```

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "pageSize": 15,
  "from": 0,
  "total": 1,
  "results": [
    {
      "auditEventUser": {
        "id": 1,
        "fullName": "testadmin@logz.io",
        "userToken": false,
        "deleted": false
      },
      "date": 1516538068641,
      "auditEventTypeTitle": "Added user",
      "ip": null,
      "geoLocation": null,
      "extraDataList": [
        {
          "fieldName": "Name",
          "oldValue": null,
          "newValue": "a@a.com"
        }
      ],
      "valid": true
    }
  ],
  "auditEventUsersList": [
    {
      "id": 1,
      "fullName": "testadmin@logz.io",
      "userToken": false,
      "deleted": false
    }
  ],
  "auditEventTypesList": [
    {
      "auditEventType": "ADD_USER",
      "auditEventTypeTitle": "Added user"
    }
  ]
}
```

***EventsTypes***
----
  Return a list of event types

* **URL**

  https://api.logz.io/v1/audit-trail/event-types

* **HTTP Method:**

  `POST`

* **Request:**

***example***
```json
{
}
```

* **Swagger Specfication**
```json
{
    "/audit-trail/event-types": {
        "post": {
            "consumes": [
                "application/json"
            ],
            "produces": [
                "application/json"
            ],
            "parameters": [
            ],
            "responses": {
                "200": {
                    "description": "OK",
                    "headers": {
                    },
                    "schema": {
                        "$ref": "#/definitions/AuditTrailEventTypesResponse"
                    }
                }
            },
            "tags": [
                "eventTypes"
            ]
        }
    },
    
    "AuditTrailEventTypesResponse": {
        "properties": {
            "eventTypes": {
                "type": "array",
                "items": {
                    "type": "string"
                }
            }
        }
    }
}
```

* **Success Response**

**Code:** 200 OK

**Content:** 
  
```json
{
  "eventTypes": [
    "Added user",
    "Admin created a sub account",
    "Admin created a timeless index",
    "Admin deleted a sub account",
    "Admin deleted timeless index",
    "Admin disabled LogSize setting field",
    "Admin disabled account utilization indexing",
    "Admin enabled LogSize setting field",
    "Admin enabled account utilization indexing",
    "Admin has changed permissions for Logz.io support access",
    "Admin updated a sub account",
    "Admin updated timeless index",
    "Changed password",
    "Deleted user",
    "Failed login",
    "Login"
    ]
}
```
