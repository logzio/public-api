# Scroll API

## General
The Scroll API allows Logz.io users to run queries on the data in their account via REST (i.e. HTTP). The query language used is Elasticsearch Search API DSL, with certain limitations. We highly recommend you read the restrictions listed below (In the Request Body section) before beginning to use the API

## License 
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.

The number of results are limited to 1,000 per page response.

The number of results returned is limited by the account's plan and calculated on a daily basis.

## Scroll Command

### URL
URL: https://api.logz.io/v1/scroll (US Accounts)
     https://api-eu.logz.io/v1/scroll (EU Accounts)
* Make sure to use https and not http, otherwise you will get a '403 Forbidden' response.

### HTTP Headers
* "X-API-TOKEN" -  The value should be the API token you generated (as explained in the License section above)
* "Content-Type" - "application/json"  (required)

### Request Body 
Elasticsearch Scroll API Body as documented  in [Elaticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/search-request-scroll.html), with the following restrictions:
* We support the following top-level elements in the Elasticsearch Search API:  `query`, `from`, `size`, `sort`, `_source`, `post_filter`, `scroll`, `scroll_id`.
* When using the `query_string` element, you are not allowed to set its field named `allow_leading_wildcard` to true
* When using the `wildcard` element, you are not allowed to have its value start with `*` or `?`
* When using the `scroll_id` element, you are not allowed to use `query` element.
* Your query will only run on data you sent today and yesterday. You can still specify a filter on the timestamp field to allow smaller time frame.
* `scroll` element supports the following time units seconds/minutes/hours/days from [Time-Unit] (https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#time-units).
* When specifying the `scroll` element the value cannot exceed 5 minutes.
* When not specifying the `scroll` element the deafult value of 1 minute will be used.
* You can not sort on the `message` field
* You can not use the `fuzzy_max_expansions` or `max_expansions` elements inside the `query` element
* You can not use the `max_determinized_states` element inside the `query` element

### Response

----------------------
| Status code | Body    | Meaning |
| ----------- | ------- | ------- | 
| 200         | The result of the query, as documented in Elasticsearch Search API (link above) | Query succeeded |
| 400         | "Found JSON elements which are not allowed [...]" | See Limits above for list of allowed elements in Query |
| 400         | "[...] query_string.allow_leading_wildcard not allowed to be true" | |
| 400         | "[...] 'wildcard' queries are not allowed to start with '*' or '?'" | |
| 400         | "ScrollId and query parameter can not co-exist in request" | |
| 400         | "Scroll can't exceed [...]" | |
| 400         | "[...] contains a wildcard search with leading wildcard which is not allowed [...]" | |
| 400         | "Query has failed. Elasticsearch Response: [...]" | Elasticsearch failed running this search request. See the Elasticsearch response to understand what went wrong. Most likely the syntax of the search request does not conform with the search API DSL - see link above for full documentation |
| 400         | "Query syntax (in 'query' field) is invalid: [...]" | |
| 400         | "This search can't be executed: [...]. Please contact customer support for more details" | Something unexpected in the query prevented him from running. Contact our customer support for help | 
| 403         |  | Your account is not allowed to use Scroll API. Please contact support/sales to enable this feature for you|
| 429         |  | Exceeded your account rate limit of searches |
| 429         | "Your account is allowed to run up to __ concurrent queries via the API" | Your account is allowed to run up to X concurrent queries via the Scroll API. This error means you've exceeded this limit |
| 503         |  | Our service is temporarily unavailable. You should try again when that happens. |
| All other codes | A message that explain the specific error | Query failures |
----------------------


## Example

### Initial Search
Create a file named `query.json` and place the following query in it:
```json
{
	"size": 1,
	"scroll": "1m",
	"query": {
		"bool": {
			"must": [{
				"range": {
					"@timestamp": {
						"gte": "now-5m",
						"lte": "now"
					}
				}
			}]
		}
	}
}
```
This query returns scroll_id and 1 document from the last 5 minutes

Run the following command:
```bash
curl -XPOST 'https://api.logz.io/v1/scroll'  --header "X-API-TOKEN: your-api-access-token" --header "Content-Type: application/json" --data-binary @query.json
```

Make sure to replace `your-api-access-token` with the token you created as instructed in License section

Example result:

```json
{
  "code": 200,
  "scrollId": "DnF1ZXJ5VGhlbkZldGNoAgAAAAAAN0JtFkNnX3lDWUFqU0R5TkNqQ3JPdm55ancAAAAAADdCbhZDZ195Q1lBalNEeU5DakNyT3ZueWp3",
  "hits": {
  	"total":20,
    "max_score":1.0,
	"hits":
	  [{
	  	"_index":"index_v9",
	  	"_type":"t1",
	  	"_score":1.0,
	  	"_source":{
	  		"message":"hello world",
	  		"type":"t2",
	  		"@timestamp":"2017-11-08T12:32:38.603Z"
	  	}
	  }]
	}
}
```

### Next Batch
Create a file named `batch.json` and place the following in it:
```json
{
	"size": 1,
	"scroll": "1m",
	"scroll_id": "DnF1ZXJ5VGhlbkZldGNoAgAAAAAAN0JtFkNnX3lDWUFqU0R5TkNqQ3JPdm55ancAAAAAADdCbhZDZ195Q1lBalNEeU5DakNyT3ZueWp3",
}
```
This query returns new scroll_id and the next document from the last 5 minutes

The scroll_id parameter should contain the scrollId that was return in the previous result.   

Run the following command:
```bash
curl -XPOST 'https://api.logz.io/v1/scroll'  --header "X-API-TOKEN: your-api-access-token" --header "Content-Type: application/json" --data-binary @batch.json
```

Make sure to replace `your-api-access-token` with the token you created as instructed in License section

Example result:

```json
{
  "code": 200,
  "scrollId": "DnF1ZXJ5VGhlbkZldGNoAgAAAAAAN0JtFkNnX3lDWUFqU0R5TkNqQ3JPdm55ancAAAAAADdCbhZDZ195Q1lBalNEeU5DakNyT3ZueWp4",
  "hits": {
  	"total":20,
    "max_score":1.0,
	"hits":
	  [{
	  	"_index":"index_v9",
	  	"_type":"t1",
	  	"_score":1.0,
	  	"_source":{
	  		"message":"hello world",
	  		"type":"t2",
	  		"@timestamp":"2017-11-08T12:32:40.603Z"
	  	}
	  }]
	}
}
```
