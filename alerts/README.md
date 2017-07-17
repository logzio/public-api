# Alerts API

## General
The Alerts API allows creating, updating, reading and deleting alerts.
During a create or update operation the alert structure is validated.
The API also allows fetching an account notification endpoints.

## Limitations
Using the API requires an API token which can be generated here - https://app.logz.io/#/dashboard/account/tokens 

The number of simultaneous api calls is controlled and limited by Logz.io.

The number of total active alerts in account is limited by the account plan.

### Examples:
## Execute a create alert call

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
    "filter": "{\"bool\":{\"must\":[{\"range\":{\"@timestamp\":{\"gte\":1472454810934,\"lte\":1472458410934}}}],\"must_not\":[]}}",
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

### query_string format
See  [Query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax)

### API specification in Swagger format
```json
{
	"swagger": "2.0",
	"info": {
		"version": "1.0.0",
		"title": "Services"
	},
	"paths": {
		"/alerts": {
			"get": {
				"summary": "Get all alerts",
				"produces": ["application/json"],
				"responses": {
					"200": {
						"description": "successful operation",
						"schema": {
							"type": "array",
							"items": {
								"$ref": "#/definitions/AlertResponse"
							}
						}
					}
				}
			},
			"post": {
				"summary": "Create an alert",
				"produces": ["application/json"],
				"parameters": [{
					"in": "body",
					"name": "body",
					"required": true,
					"schema": {
						"$ref": "#/definitions/AlertRequest"
					}
				}],
				"responses": {
					"200": {
						"description": "successful operation",
						"schema": {
							"$ref": "#/definitions/AlertResponse"
						}
					}
				}
			}
		},
		"/alerts/notification-endpoints": {
			"get": {
				"summary": "Get all of the account notification endpoints",
				"produces": ["application/json"],
				"responses": {
					"200": {
						"description": "successful operation",
						"schema": {
							"type": "array",
							"items": {
								"$ref": "#/definitions/NotificationEndpointResponse"
							}
						}
					}
				}
			}
		},
		"/alerts/{alertId}": {
			"get": {
				"summary": "Get an alert",
				"produces": ["application/json"],
				"parameters": [{
					"name": "alertId",
					"in": "path",
					"required": true,
					"type": "integer",
					"format": "int32"
				}],
				"responses": {
					"200": {
						"description": "successful operation",
						"schema": {
							"$ref": "#/definitions/AlertResponse"
						}
					}
				}
			},
			"put": {
				"summary": "Update an alert",
				"produces": ["application/json"],
				"parameters": [{
					"name": "alertId",
					"in": "path",
					"required": true,
					"type": "integer",
					"format": "int32"
				}, {
					"in": "body",
					"name": "body",
					"required": true,
					"schema": {
						"$ref": "#/definitions/AlertRequest"
					}
				}],
				"responses": {
					"200": {
						"description": "successful operation",
						"schema": {
							"$ref": "#/definitions/AlertResponse"
						}
					}
				}
			},
			"delete": {
				"summary": "Delete an alert",
				"produces": ["application/json"],
				"parameters": [{
					"name": "alertId",
					"in": "path",
					"required": true,
					"type": "integer",
					"format": "int32"
				}],
				"responses": {
					"default": {
						"description": "successful operation"
					}
				}
			}
		},
		"definitions": {
			"AlertResponse": {
				"type": "object",
				"properties": {
					"alertId": {
						"type": "integer",
						"format": "int32"
					},
					"lastUpdated": {
						"type": "string",
						"format": "date-time"
					},
					"lastUpdatedBy": {
						"type": "string"
					},
					"severity": {
						"type": "string",
						"enum": ["LOW", "MEDIUM", "HIGH"]
					},
					"title": {
						"type": "string"
					},
					"description": {
						"type": "string"
					},
					"query_string": {
						"type": "string"
					},
					"filter": {
						"type": "string"
					},
					"operation": {
						"type": "string",
						"enum": ["LESS_THAN", "GREATER_THAN", "LESS_THAN_OR_EQUALS", "GREATER_THAN_OR_EQUALS", "EQUALS", "NOT_EQUALS"]
					},
					"threshold": {
						"type": "number",
						"format": "double"
					},
					"searchTimeFrameMinutes": {
						"type": "integer",
						"format": "int32"
					},
					"notificationEmails": {
						"type": "array",
						"items": {
							"type": "string"
						}
					},
					"isEnabled": {
						"type": "boolean"
					},
					"suppressNotificationsMinutes": {
						"type": "integer",
						"format": "int32"
					},
					"valueAggregationType": {
						"type": "string",
						"enum": ["SUM", "MIN", "MAX", "AVG", "COUNT", "NONE"]
					},
					"valueAggregationField": {
						"type": "string"
					},
					"groupByAggregationFields": {
						"type": "array",
						"items": {
							"type": "string"
						}
					},
					"alertNotificationEndpoints": {
						"type": "array",
						"items": {
							"type": "integer",
							"format": "int32"
						}
					}
				}
			},
			"AlertRequest": {
				"type": "object",
				"required": ["notificationEmails", "query_string", "severity", "title"],
				"properties": {
					"severity": {
						"type": "string",
						"enum": ["LOW", "MEDIUM", "HIGH"]
					},
					"title": {
						"type": "string"
					},
					"description": {
						"type": "string"
					},
					"query_string": {
						"type": "string"
					},
					"filter": {
						"type": "string"
					},
					"operation": {
						"type": "string",
						"enum": ["LESS_THAN", "GREATER_THAN", "LESS_THAN_OR_EQUALS", "GREATER_THAN_OR_EQUALS", "EQUALS", "NOT_EQUALS"]
					},
					"threshold": {
						"type": "number",
						"format": "double"
					},
					"searchTimeFrameMinutes": {
						"type": "integer",
						"format": "int32"
					},
					"notificationEmails": {
						"type": "array",
						"items": {
							"type": "string"
						}
					},
					"isEnabled": {
						"type": "boolean",
                        "default": true
					},
					"suppressNotificationsMinutes": {
						"type": "integer",
						"format": "int32",
						"minimum": 5.0
					},
					"valueAggregationType": {
						"type": "string",
						"enum": ["SUM", "MIN", "MAX", "AVG", "COUNT", "NONE"]
					},
					"valueAggregationField": {
						"type": "string"
					},
					"groupByAggregationFields": {
						"type": "array",
						"items": {
							"type": "string"
						},
						"maxItems": 1,
						"minItems": 0
					},
					"alertNotificationEndpoints": {
						"type": "array",
						"items": {
							"type": "integer",
							"format": "int32"
						}
					}
				}
			},
			"NotificationEndpointResponse": {
				"type": "object",
				"properties": {
					"id": {
						"type": "integer",
						"format": "int32"
					},
					"type": {
						"type": "string"
					},
					"description": {
						"type": "string"
					},
					"title": {
						"type": "string"
					},
					"createdDate": {
						"type": "string",
						"format": "date-time"
					},
					"modifiedDate": {
						"type": "string",
						"format": "date-time"
					}
				}
			}
		}
	}
}
```

### Errors

> {
>   "code": 400
> }

> {
>   "code": 404
> }