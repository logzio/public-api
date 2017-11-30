# Shared Tokens API

## General
The Shared Tokens API allows creating, updating, reading and deleting tokens that can be used to share dashboards.
It also enable creating, deleting and reading query filters to filter on the queries done using the tokens

## License
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens

## Limitations
Using the Shared Tokens API requires special permission from logz.io.

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


## Execute a get all shared tokens call

```
$ curl -XPOST 'https://api.logz.io/shared-tokens'
  -d '{
        "post": {
          "tokenName": "string",
          "filters": [
            2
          ]
        }
    }'```

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

## Execute a get all shared tokens call

```
$ curl -XPOST 'https://api.logz.io/shared-tokens'
  -d '{
       "get": {
       }
    }'```

Sample response:
```
[
  {
    "id": 7,
    "name": "string",
    "token": "string",
    "filterIds": [
      2
    ]
  }
]
```


### Errors

{
  "code": 400
}

{
   "code": 404
}
