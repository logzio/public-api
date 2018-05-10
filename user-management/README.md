# User Management API

## General
The User Management API allows creating, updating, reading and deleting of users  

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens
In addition, using the User Management API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***List Users***
----
  Return list of configured account users.

* **URL**

  https://api.logz.io/v1/user-management

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
---
* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
[
  {
    "username": "example@a.com",
    "fullName": "example",
    "accountID": 4,
    "roles": [
      3
    ],
    "active": true
  }
]
```

* **Swagger Specification**
```json
{
   "paths" : {
      "/user-management": {
         "get": {
           "tags": [
             "User Management"
           ],
           "summary": "List users",
           "description": "",
           "operationId": "listUsers",
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
                   "$ref": "#/definitions/User"
                 }
               }
             }
           }
         }
      }
   },
   "definitions" : {
        "User": {
          "type": "object",
          "properties": {
            "id": {
              "type": "integer",
              "format": "int32"
            },
            "username": {
              "type": "string"
            },
            "fullName": {
              "type": "string"
            },
            "accountID": {
              "type": "integer",
              "format": "int32"
            },
            "roles": {
              "type": "array",
              "items": {
                "type": "integer",
                "format": "int32"
              }
            },
            "active": {
              "type": "boolean"
            }
          }
        }   
   }
}
```

***Create User***
----
  Create a user.

* **URL**

  https://api.logz.io/v1/user-management

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| username| string| username in an email format|
| fullname| string| |
| accountId| int| The account id the user will be attached to |
| roles| array| Array of role ids (use 2 for regular user and 3 for account admin)|
---

* **Example**
```json
{
  "username": "example@a.com",
  "fullName": "example",
  "accountID": 4,
  "roles": [
    3
  ]
}
```
* **Success Response:**

**Code:** 200 OK

---
| Parameter|Type|Description|
|---|---|---|
| id| int| The id of the user|
---

  
**Content:** 
```json
{
  "id": 1
}
```

* **Swagger Specification**
```json
{
   "paths" : {
      "/user-management": {
        "post": {
          "tags": [
            "User Management"
          ],
          "summary": "Create user",
          "description": "",
          "operationId": "createUser",
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
                "$ref": "#/definitions/UserManagementUpsertRequest"
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
                "$ref": "#/definitions/UserManagementUpsertResponse"
              }
            }
          }
        }
      }
   },
   "definitions" : {
       "UserManagementUpsertResponse": {
         "type": "object",
         "properties": {
           "id": {
             "type": "integer",
             "format": "int32"
           }
         }
       },
       "UserManagementUpsertRequest": {
         "type": "object",
         "required": [
           "fullName",
           "username"
         ],
         "properties": {
           "username": {
             "type": "string",
             "pattern": "^[_A-Za-z0-9-\\+]+(\\.[_A-Za-z0-9-]+)*@[A-Za-z0-9-]+(\\.[A-Za-z0-9-]+)*(\\.[A-Za-z]{2,})$"
           },
           "fullName": {
             "type": "string"
           },
           "accountID": {
             "type": "integer",
             "format": "int32"
           },
           "roles": {
             "type": "array",
             "items": {
               "type": "integer",
               "format": "int32"
             }
           }
         }
       }
   } 
}
```

***Get User***
----
  Return user by id.

* **URL**

  https://api.logz.io/v1/user-management/{id}

* **HTTP Method:**

  `GET`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| User Id|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
    "id": 1,
    "username": "testadmin@logz.io",
    "fullName": "testadmin@logz.io",
    "accountID": 1,
    "roles": [
      3
    ],
    "active": true
}
```

* **Swagger Specification**
```json
{
  "paths": {
    "/user-management/{id}": {
      "get": {
        "tags": [
          "User Management"
        ],
        "summary": "Get user",
        "description": "",
        "operationId": "getUser",
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
              "$ref": "#/definitions/User"
            }
          }
        }
      }
    }
  }
}
```

***Update User***
----
  Update user.

* **URL**

  https://api.logz.io/v1/user-management/{id}

* **HTTP Method:**

  `PUT`

---
| Parameter|Type|Description|
|---|---|---|
| username| string| username in an email format|
| fullname| string| |
| accountId| int| The account id the user will be attached to |
| roles| array| Array of role ids (use 2 for regular user and 3 for account admin)|
---

* **Example**
```json
{
  "username": "example@a.com",
  "fullName": "example",
  "accountID": 4,
  "roles": [
    3
  ]
}
```
* **Success Response:**

**Code:** 200 OK

---
| Parameter|Type|Description|
|---|---|---|
| id| int| The id of the user|
---

  
**Content:** 
```json
{
  "id": 1
}
```

* **Swagger Specification**
```json
{
   "paths" : {
       "/user-management/{id}": {
          "put": {
            "tags": [
              "User Management"
            ],
            "summary": "Update user",
            "description": "",
            "operationId": "updateUser",
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
                  "$ref": "#/definitions/UserManagementUpsertRequest"
                }
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
              },
              {
                "name": "X-AUTH-TOKEN",
                "in": "header",
                "description": "Authentication User Session",
                "required": false,
                "type": "string"
              }
            ],
            "responses": {
              "200": {
                "description": "successful operation",
                "schema": {
                  "$ref": "#/definitions/UserManagementUpsertResponse"
                }
              }
            }
          }
       }
   },
   "definitions" : {
       "UserManagementUpsertResponse": {
         "type": "object",
         "properties": {
           "id": {
             "type": "integer",
             "format": "int32"
           }
         }
       },
       "UserManagementUpsertRequest": {
         "type": "object",
         "required": [
           "fullName",
           "username"
         ],
         "properties": {
           "username": {
             "type": "string",
             "pattern": "^[_A-Za-z0-9-\\+]+(\\.[_A-Za-z0-9-]+)*@[A-Za-z0-9-]+(\\.[A-Za-z0-9-]+)*(\\.[A-Za-z]{2,})$"
           },
           "fullName": {
             "type": "string"
           },
           "accountID": {
             "type": "integer",
             "format": "int32"
           },
           "roles": {
             "type": "array",
             "items": {
               "type": "integer",
               "format": "int32"
             }
           }
         }
       }
   }
}
```

***Delete User***
----
  Delete a user by id.

* **URL**

  https://api.logz.io/v1/user-management/{id}

* **HTTP Method:**

  `DELETE`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| id| int| User Id|
---

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
```

* **Swagger Specification**
```json
{
   "paths" : {
       "/user-management/{id}": {
          "delete": {
            "tags": [
              "User Management"
            ],
            "summary": "Delete user",
            "description": "",
            "operationId": "deleteUser",
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
              "default": {
                "description": "successful operation"
              }
            }
          }
        }
   }
}
```

***Suspend User***
----
  Suspend user.

* **URL**

  https://api.logz.io/v1/user-management/suspend/{id}

* **HTTP Method:**

  `POST`

---
| Parameter|Type|Description|
|---|---|---|
| id| int| User id|
---

* **Example**
```json
{
  "id": 3
}
```
* **Success Response:**

**Code:** 204 OK

  
**Content:** 
```json
```

* **Swagger Specification**
```json
{
   "paths" : {
      "/user-management/suspend/{id}": {
         "post": {
           "tags": [
             "User Management"
           ],
           "summary": "Suspend user",
           "description": "",
           "operationId": "suspendUser",
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
             "default": {
               "description": "successful operation"
             }
           }
         }
      }
   },
   "definitions" : {
   }
}
```

***Unsuspend User***
----
  Unsuspend user.

* **URL**

  https://api.logz.io/v1/user-management/unsuspend/{id}

* **HTTP Method:**

  `POST`

---
| Parameter|Type|Description|
|---|---|---|
| id| int| User id|
---

* **Example**
```json
{
  "id": 3
}
```
* **Success Response:**

**Code:** 204 OK

  
**Content:** 
```json
```

* **Swagger Specification**
```json
{
   "paths" : {
      "/user-management/unsuspend/{id}": {
         "post": {
           "tags": [
             "User Management"
           ],
           "summary": "Unsuspend user",
           "description": "",
           "operationId": "unsuspendUser",
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
             "default": {
               "description": "successful operation"
             }
           }
         }
      }
   },
   "definitions" : {
   }
}
```

### API specification in Swagger format
[JSON file here](swagger.json)
