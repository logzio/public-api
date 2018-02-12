# Shared Tokens API

## General
The Shared Tokens API allows creating, updating, reading and deleting tokens that can be used to share dashboards and snapshots. It also enables creating, deleting and reading token filters used to filter queries executed when using tokens

## License & Permissions
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens

In addition, using the Shared Tokens API requires using a an API token with a dedicated permission. To receive such a permission, please reach out to Logz.io support.

## Limitations
The number of simultaneous api calls is controlled and limited by Logz.io.

The number of total shared tokens in account is limited by by Logz.io.

### Request Header:
- X-API-TOKEN : contains a token in order to access the API, which can be generated here - https://app.logz.io/#/dashboard/account/tokens
- "Content-Type" - "application/json" (required)

### Response

----------------------
| Status code | Body    | Meaning |
| ----------- | ------- | ---- |
| 200         | The shared tokens requested, created or updated | Succeeded |
| 400         | The shared token or query filter not found
| 403         |  | Your account is not allowed to use Shared Tokens API. Please contact support/sales to enable this feature for you|
| 503         |  | Our service is temporarily unavailable. You should try again when that happens. |
| All other codes | A message that explain the specific error
----------------------


### Examples:


## Execute a create shared token call

```
$ curl -XPOST 'https://api.logz.io/v1/shared-tokens'
  --header "X-API-TOKEN : your-api-access-token"
  --header "Content-Type: application/json"
  -d '{
        "post": {
          "tokenName": "string",
          "filters": [
            2
          ]
        }
    }'
```

Sample response:

```
  {
    "id": 7,
    "name": "string",
    "token": "string",
    "filterIds": [
      2
    ]
  }
```

### API specification in Swagger format
```json
{
  "swagger": "2.0",
  "info": {
    "version": "1.0.0",
    "title": "Services"
  },
  "paths": {
    "/shared-tokens": {
      "get": {
        "operationId": "getAllTokens",
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
              "type": "array",
              "uniqueItems": true,
              "items": {
                "$ref": "#/definitions/SharedToken"
              }
            },
            "headers": {

            }
          }
        }
      },
      "post": {
        "operationId": "createToken",
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
              "$ref": "#/definitions/CreateSharedTokenRequest"
            }
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
              "$ref": "#/definitions/SharedToken"
            },
            "headers": {

            }
          }
        }
      }
    },
    "/shared-tokens/{id}": {
      "get": {
        "operationId": "getToken",
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
              "$ref": "#/definitions/SharedToken"
            },
            "headers": {

            }
          }
        }
      },
      "put": {
        "operationId": "updateToken",
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
              "type": "array",
              "items": {
                "type": "integer",
                "format": "int32"
              }
            }
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
              "$ref": "#/definitions/SharedToken"
            },
            "headers": {

            }
          }
        }
      },
      "delete": {
        "operationId": "deleteToken",
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
          "default": {
            "description": "successful operation"
          }
        }
      }
    },
    "/shared-tokens/filters": {
      "get": {
        "operationId": "getAllFilters",
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
              "type": "array",
              "uniqueItems": true,
              "items": {
                "$ref": "#/definitions/QueryFilter"
              }
            },
            "headers": {

            }
          }
        }
      },
      "post": {
        "operationId": "createFilter",
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
              "$ref": "#/definitions/QueryFilter"
            }
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
              "$ref": "#/definitions/QueryFilter"
            },
            "headers": {

            }
          }
        }
      }
    },
    "/shared-tokens/filters/{id}": {
      "get": {
        "operationId": "getFilter",
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
              "$ref": "#/definitions/QueryFilter"
            },
            "headers": {

            }
          }
        }
      },
      "delete": {
        "operationId": "deleteFilter",
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
          "default": {
            "description": "successful operation"
          }
        }
      }
    }
  }
}
```
