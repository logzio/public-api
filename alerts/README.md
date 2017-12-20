# Alerts API

## General
The Alerts API allows creating, updating, reading and deleting alerts.
During a create or update operation the alert structure is validated.
The API also allows fetching an account notification endpoints.

## Limitations
Using the API requires an API token which can be generated here - https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of simultaneous api calls is controlled and limited by Logz.io.

The number of total active alerts in account is limited by the account plan.

## Endpoints

**Create Alert**
----
POST https://api.logz.io/v1/alerts<br />
<br />
Expects AlertRequest as body, and creates a new alert.<br />

**Update Alert**
----
PUT http://api.logz.io/v1/alerts/:id <br />
<br />
Expects AlertRequest as body, updates an existing alert by id.<br />
Currently does not support partial updates, so it should be used with the same parameters as the create endpoint.

**Get Alert By ID**
----
GET http://api.logz.io/v1/alerts/:id

**Get All Alerts**
----
GET http://api.logz.io/v1/alerts

### Examples:
## Execute a create alert call
Update calls should be done with the same parameters, as partial updates are not currently supported.
```
$ curl -XPOST 'https://api.logz.io/v1/alerts'  
  --header "X-API-TOKEN : your-api-access-token" 
  --header "Content-Type: application/json"
  -d '{
        "severity": "LOW",
        "title": "Error level logs",
        "description": "Capture ERROR level logs in the given time range",
        "query_string": "loglevel:ERROR",
        "filter": "{\"bool\":{\"must\":[{\"match\":{\"type\":\"mytype\"}}],\"must_not\":[]}}",
        "operation": "GREATER_THAN",
        "threshold": 1,
        "searchTimeFrameMinutes": 5,
        "notificationEmails": [
          "alerts@my.organization.com"
        ],
        "isEnabled": true,
        "suppressNotificationsMinutes": 0,
        "valueAggregationType": "NONE",
        "valueAggregationField": null,
        "groupByAggregationFields": [],
        "alertNotificationEndpoints": [
      	    0
        ]
      }'
```

Sample response:
```
{
    "alertId": 63,
    "lastUpdatedBy": "myuser@my.organization.com",
    "lastUpdated": "2017-07-16'T'12:22:28.301'Z'",
    "severity": "LOW",
    "title": "Error level logs",
    "description": "Capture ERROR level logs in the given time range",
    "query_string": "loglevel:ERROR",
    "filter": "{\"bool\":{\"must\":[{\"match\":{\"type\":\"mytype\"}}],\"must_not\":[]}}",
    "operation": "GREATER_THAN",
    "threshold": 1,
    "searchTimeFrameMinutes": 5,
    "notificationEmails": [
      "me@my.organization.com"
    ],
    "isEnabled": true,
    "suppressNotificationsMinutes": 0,
    "valueAggregationType": "NONE",
    "valueAggregationField": null,
    "groupByAggregationFields": [],
    "alertNotificationEndpoints": [
        0
    ]
}
```

### Request Header:
- X-API-TOKEN : contains a token in order to access the API, which can be generated here - https://app.logz.io/#/dashboard/account/tokens 
- "Content-Type" - "application/json" (required)

### query_string format
See  [Query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)

## API specification in Swagger format
[JSON file here](swagger.json)

### Errors
400 - Bad request, if the alert definition isn't valid.
403 - Unauthorized, if an invalid token header was sent.
404 - Not found, if the alert id was not found.