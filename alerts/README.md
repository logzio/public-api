# Alerts API

## General
The Alerts API allows creating, updating, reading and deleting alerts.
During a create or update operation the alert structure is validated.
The API also allows fetching an account notification endpoints.

## Limitations
Using the API requires an API token which can be generated here - https://app.logz.io/#/dashboard/settings/api-tokens

The number of simultaneous api calls is controlled and limited by Logz.io.

The number of total active alerts in account is limited by the account plan.

## Endpoints
Replace the URLs below 'api.logz.io' part with your region's API host. For more information, see [Account Region](https://docs.logz.io/user-guide/accounts/account-region.html#regions-and-urls).

**Create Alert** <br />
POST https://api.logz.io/v1/alerts<br />
<br />
Expects AlertRequest as body, and creates a new alert.<br />
You can find the expected structure in the examples section.

**Update Alert** <br />
PUT https://api.logz.io/v1/alerts/:id <br />
<br />
Expects AlertRequest as body, updates an existing alert by id.<br />
Currently does not support partial updates, so it should be used with the same parameters as the create endpoint.

**Get Alert By ID** <br />
GET https://api.logz.io/v1/alerts/:id

**Delete Alert By ID** <br />
DELETE https://api.logz.io/v1/alerts/:id

**Get Filtered Triggered Alerts** <br />
Expects TriggeredAlertsRequest as filter body, and returns paged filtered list of triggered alerts. <br/>
You can find the expected structure of request in the examples section.
* **URL**

  http://api.logz.io/v1/alerts/triggered-alerts

* **HTTP Method:**

  `POST`

* **Request:**
---
| Parameter|Type|Description|
|---|---|---|
| from| integer| |
| size| integer| Size of page to return|
| search| string| Part of the alert name to filter by name (ignore case)|
| severities| array of enum| Filter by triggered severities (SEVERE/HIGH/MEDIUM/LOW/INFO) of alerts
| sortBy| enum| DATE/SEVERITY|
| sortOrder| enum| ASC/DESC|
| tags| array of string| List of tags the alert is related to|
---
<br/>

**Get All Alerts** <br />
GET http://api.logz.io/v1/alerts

## Examples:
### Create Alert
Update calls should be done with the same parameters, as partial updates are not currently supported.
```
$ curl -XPOST 'https://api.logz.io/v1/alerts'  
  --header "X-API-TOKEN : your-api-access-token"
  --header "Content-Type: application/json"
  -d '{
        "title": "Error level logs",
        "description": "Capture ERROR level logs in the given time range",
        "query_string": "loglevel:ERROR",
        "filter": "{\"bool\":{\"must\":[{\"match\":{\"type\":\"mytype\"}}],\"must_not\":[]}}",
        "operation": "GREATER_THAN",
        "severityThresholdTiers": [{
        	    "severity": "HIGH",
        	    "threshold": 2
        	},
        	{
        	    "severity": "MEDIUM",
       		    "threshold": 1
        	},
        	{
        	    "severity": "LOW",
       		    "threshold": 0
        	}
        ],
        "searchTimeFrameMinutes": 5,
        "notificationEmails": [
          "alerts@my.organization.com"
        ],
        "isEnabled": true,
        "suppressNotificationsMinutes": 0,
        "valueAggregationType": "NONE",
        "valueAggregationField": null,
        "groupByAggregationFields": [],
        "alertNotificationEndpoints": []
      }'
```

Sample response:
```
{
    "alertId": 63,
    "lastUpdatedBy": "myuser@my.organization.com",
    "lastUpdated": "2017-07-16'T'12:22:28.301'Z'",
    "title": "Error level logs",
    "description": "Capture ERROR level logs in the given time range",
    "query_string": "loglevel:ERROR",
    "filter": "{\"bool\":{\"must\":[{\"match\":{\"type\":\"mytype\"}}],\"must_not\":[]}}",
    "operation": "GREATER_THAN",
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
    ],
    "severityThresholdTiers": [{
    	    "severity": "HIGH",
    	    "threshold": 2.0
    	}, {
    		"severity": "MEDIUM",
    		"threshold": 1.0
    	}, {
    		"severity": "LOW",
    		"threshold": 0.0
    	}
    ]
}
```
### Get Filtered Paged List of Triggered Alerts
```
$ curl -XPOST 'https://api.logz.io/v1/alerts/triggered-alerts'  
  --header "X-API-TOKEN : your-api-access-token"
  --header "Content-Type: application/json"
  -d '{
      	"from": 0,
      	"size": 15,
      	"search": "test",
      	"severities": ["HIGH", "LOW"],
      	"sortBy": "DATE",
      	"sortOrder": "ASC"
      }'
```
Sample response:
```
{
	"pageSize": 2,
	"from": 0,
	"total": 2,
	"results": [{
		"alertId": 2,
		"name": "Title AND OXtvb",
		"eventDate": 1523970562.691000000,
		"severity": "LOW"
	}, {
		"alertId": 1,
		"name": "Title AND FOrRr",
		"eventDate": 1523970558.657000000,
		"severity": "HIGH"
	}]
}
```

### Request Header:
- X-API-TOKEN : contains a token in order to access the API, which can be generated here - https://app.logz.io/#/dashboard/settings/api-tokens
- "Content-Type" - "application/json" (required)

### query_string format
See  [Query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)

## API specification in Swagger format
[JSON file here](swagger.json)

## Errors
400 - Bad request, if the alert definition isn't valid.<br />
403 - Unauthorized, if an invalid token header was sent.<br />
404 - Not found, if the alert id was not found.<br />
