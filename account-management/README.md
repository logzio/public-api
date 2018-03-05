# Account Management API

## General
The Account Management API allows creating, updating, reading and deleting accounts.

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens
In addition, using the Account Management API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***Create Account***
----
  Create Account

* **URL**

  https://api.logz.io/v1/account-management

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
|email|string|The account admin email address|
|accountName|string|The account name|
|maxDailyGB|float|The maximum daily usage in GB|
|retentionDays|int|The retention in days|
|searchable|boolean|Is the account searchable|
|accessible|boolean|Is the account accessible|
|sharingObjectsAccounts|array|Ids of all the accounts that can access the account data|
|docSizeSetting|boolean|Is document size attached to logs|
|utilizationSettings.utilizationEnabled|boolean|Are utilization metrics written to the logs|
|utilizationSettings.frequencyMinutes|int|How often, in minutes, utilization metrics are written to logs|

* **Example**
```json
{
  "email": "example@a.com",
  "accountName": "example",
  "maxDailyGB": 1,
  "retentionDays": 5,
  "searchable": true,
  "accessible": true,
  "sharingObjectsAccounts": [
    4
  ],
  "docSizeSetting": true,
  "utilizationSettings": {
    "frequencyMinutes": 0,
    "utilizationEnabled": true
  }
}
```
* **Success Response:**

**Code:** 200 OK

---
| Parameter|Type|Description|
|---|---|---|
|accountId|int|The account id|
|accountToken|string|The account token to be used for log shipping|
---

**Content:** 
```json
{
  "accountId": 4,
  "accountToken": "lywYbJOPuoOItOzAwCrAmyDuyuPERXAC"
}
```

* **Swagger Specification**
```json
{
   "paths": {
        "/account-management": {
            "post" : {
              "operationId" : "createTimeBasedAccount",
              "produces" : [ "application/json" ],
              "parameters" : [ {
                "in" : "body",
                "name" : "body",
                "required" : false,
                "schema" : {
                  "$ref" : "#/definitions/TimeBasedAccountUpsertRequest"
                }
              }, {
                "name" : "x-forwarded-for",
                "in" : "header",
                "required" : false,
                "type" : "string"
              } ],
              "responses" : {
                "200" : {
                  "description" : "successful operation",
                  "schema" : {
                    "$ref" : "#/definitions/TimeBasedAccountCreationResponse"
                  }
                }
              }
            }
        }
    },
    "definitions": {
        "TimeBasedAccountCreationResponse" : {
          "type" : "object",
          "properties" : {
            "accountId" : {
              "type" : "integer",
              "format" : "int32"
            },
            "accountToken" : {
              "type" : "string"
            }
          }
        },
        "TimeBasedAccountUpsertRequest" : {
          "type" : "object",
          "required" : [ "sharingObjectsAccounts" ],
          "properties" : {
            "email" : {
              "type" : "string",
              "pattern" : "^[_A-Za-z0-9-\\+]+(\\.[_A-Za-z0-9-]+)*@[A-Za-z0-9-]+(\\.[A-Za-z0-9-]+)*(\\.[A-Za-z]{2,})$"
            },
            "accountName" : {
              "type" : "string"
            },
            "maxDailyGB" : {
              "type" : "number",
              "format" : "float"
            },
            "retentionDays" : {
              "type" : "integer",
              "format" : "int32"
            },
            "searchable" : {
              "type" : "boolean"
            },
            "accessible" : {
              "type" : "boolean"
            },
            "sharingObjectsAccounts" : {
              "type" : "array",
              "items" : {
                "type" : "integer",
                "format" : "int32"
              }
            },
            "docSizeSetting" : {
              "type" : "boolean"
            },
            "utilizationSettings" : {
              "$ref" : "#/definitions/AccountUtilizationSettings"
            }
          }
        }
    }
}
```

***Get Account***
----
  Return account by id.

* **URL**

  https://api.logz.io/v1/account-management/time-based-accounts/{id}

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
|id|int|Account Id|
---

* **Success Response:**

**Code:** 200 OK
---
| Parameter|Type|Description|
|---|---|---|
|accountId|int|The account id|
|accountName|string|The account name|
|accountToken|string|The account token to be used for log shipping|
|maxDailyGB|float|The maximum daily usage in GB|
|retentionDays|int|The retention in days|
|searchable|boolean|Is the account searchable|
|accessible|boolean|Is the account accessible|
|docSizeSetting|boolean|Is document size attached to logs|
|sharingObjectsAccounts|array|Ids of all the accounts that can access the account data|
|utilizationSettings.utilizationEnabled|boolean|Are utilization metrics written to the logs|
|utilizationSettings.frequencyMinutes|int|How often, in minutes, utilization metrics are written to logs|
---
  
**Content:** 
```json

{
  "accountId": 4,
  "accountName": "example",
  "accountToken": "lywYbJOPuoOItOzAwCrAmyDuyuPERXAC",
  "maxDailyGB": 1,
  "retentionDays": 1,
  "searchable": true,
  "accessible": true,
  "docSizeSetting": false,
  "sharingObjectsAccounts": [],
  "utilizationSettings": {
    "frequencyMinutes": null,
    "utilizationEnabled": false
  }
}

```

* **Swagger Specification**
```json
{
  "paths": {
     "/account-management/time-based-accounts/{id}" : {
        "get" : {
          "operationId" : "get",
          "produces" : [ "application/json" ],
          "parameters" : [ {
            "name" : "id",
            "in" : "path",
            "required" : true,
            "type" : "integer",
            "format" : "int32"
          } ],
          "responses" : {
            "200" : {
              "description" : "successful operation",
              "schema" : {
                "$ref" : "#/definitions/TimeBasedAccount"
              }
            }
          }
        }
     }
  },
  "definitions": {
    "TimeBasedAccount" : {
      "type" : "object",
      "properties" : {
        "accountId" : {
          "type" : "integer",
          "format" : "int32"
        },
        "accountName" : {
          "type" : "string"
        },
        "accountToken" : {
          "type" : "string"
        },
        "maxDailyGB" : {
          "type" : "number",
          "format" : "float"
        },
        "retentionDays" : {
          "type" : "integer",
          "format" : "int32"
        },
        "searchable" : {
          "type" : "boolean"
        },
        "accessible" : {
          "type" : "boolean"
        },
        "docSizeSetting" : {
          "type" : "boolean"
        },
        "sharingObjectsAccounts" : {
          "type" : "array",
          "items" : {
            "$ref" : "#/definitions/SharingAccount"
          }
        },
        "utilizationSettings" : {
          "$ref" : "#/definitions/AccountUtilizationSettings"
        }
      }
    },
    "SharingAccount" : {
      "type" : "object",
      "properties" : {
        "accountId" : {
          "type" : "integer",
          "format" : "int32"
        },
        "accountName" : {
          "type" : "string"
        }
      }
    }
  }
}
```

***Update Account***
----
  Update account by id.

* **URL**

  https://api.logz.io/v1/account-management/time-based-accounts/{id}

* **HTTP Method:**

  `PUT`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
|id|int|The account id|
|email|string|The account admin email address|
|accountName|string|The account name|
|maxDailyGB|float|The maximum daily usage in GB|
|retentionDays|int|The retention in days|
|searchable|boolean|Is the account searchable|
|accessible|boolean|Is the account accessible|
|sharingObjectsAccounts|array|Ids of all the accounts that can access the account data|
|docSizeSetting|boolean|Is document size attached to logs|
|utilizationSettings.utilizationEnabled|boolean|Are utilization metrics written to the logs|
|utilizationSettings.frequencyMinutes|int|How often, in minutes, utilization metrics are written to logs|
---

* **Example**
```json
{
  "email": "example@a.com",
  "accountName": "example",
  "maxDailyGB": 1,
  "retentionDays": 5,
  "searchable": true,
  "accessible": true,
  "sharingObjectsAccounts": [
    4
  ],
  "docSizeSetting": true,
  "utilizationSettings": {
    "frequencyMinutes": 0,
    "utilizationEnabled": true
  }
}
```

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
}
```

* **Swagger Specification**
```json
{
  "paths": {
     "/account-management/time-based-accounts/{id}" : {
          "put" : {
            "operationId" : "updateTimeBasedAccount",
            "produces" : [ "application/json" ],
            "parameters" : [ {
              "name" : "id",
              "in" : "path",
              "required" : true,
              "type" : "integer",
              "format" : "int32"
            }, {
              "in" : "body",
              "name" : "body",
              "required" : false,
              "schema" : {
                "$ref" : "#/definitions/TimeBasedAccountUpsertRequest"
              }
            }, {
              "name" : "x-forwarded-for",
              "in" : "header",
              "required" : false,
              "type" : "string"
            } ],
            "responses" : {
              "default" : {
                "description" : "successful operation"
              }
            }
          }
     }
  },
  "definitions": {
      "TimeBasedAccountCreationResponse" : {
        "type" : "object",
        "properties" : {
          "accountId" : {
            "type" : "integer",
            "format" : "int32"
          },
          "accountToken" : {
            "type" : "string"
          }
        }
      },
      "TimeBasedAccountUpsertRequest" : {
        "type" : "object",
        "required" : [ "sharingObjectsAccounts" ],
        "properties" : {
          "email" : {
            "type" : "string",
            "pattern" : "^[_A-Za-z0-9-\\+]+(\\.[_A-Za-z0-9-]+)*@[A-Za-z0-9-]+(\\.[A-Za-z0-9-]+)*(\\.[A-Za-z]{2,})$"
          },
          "accountName" : {
            "type" : "string"
          },
          "maxDailyGB" : {
            "type" : "number",
            "format" : "float"
          },
          "retentionDays" : {
            "type" : "integer",
            "format" : "int32"
          },
          "searchable" : {
            "type" : "boolean"
          },
          "accessible" : {
            "type" : "boolean"
          },
          "sharingObjectsAccounts" : {
            "type" : "array",
            "items" : {
              "type" : "integer",
              "format" : "int32"
            }
          },
          "docSizeSetting" : {
            "type" : "boolean"
          },
          "utilizationSettings" : {
            "$ref" : "#/definitions/AccountUtilizationSettings"
          }
        }
      }

  }
}
```

***Delete Account***
----
  Delete an account by id.

* **URL**

  https://api.logz.io/v1/account-management/time-based-accounts/{id}

* **HTTP Method:**

  `DELETE`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
|id|int|Account Id|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
```

* **Swagger Specification**
```json
{
  "paths": {
     "/account-management/time-based-accounts/{id}" : {
        "delete" : {
            "operationId" : "deleteTimeBasedAccount",
            "produces" : [ "application/json" ],
            "parameters" : [ {
              "name" : "id",
              "in" : "path",
              "required" : true,
              "type" : "integer",
              "format" : "int32"
            }, {
              "name" : "x-forwarded-for",
              "in" : "header",
              "required" : false,
              "type" : "string"
            } ],
            "responses" : {
              "default" : {
                "description" : "successful operation"
              }
            }
        }
     }
  }
}
```

### API specification in Swagger format
[JSON file here](swagger.json)