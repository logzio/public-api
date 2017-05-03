# Search API

## General
The Search API allows Logz.io users to run queries on the data in their account via REST (i.e. HTTP). The query language used is Elasticsearch Search API DSL, with certain limitations. We highly recommend you read the restrictions listed below (In the Request Body section) before beginning to use the API

## License 
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/account/tokens 

The number of queries executed is controlled and limited by Logz.io.

The number of results are limited to 10,000.

## Search Command

### URL
URL: https://api.logz.io/v1/search﻿
* Make sure to use https and not http, otherwise you will get a '403 Forbidden' response..

### HTTP Headers
* "X-USER-TOKEN" -  The value should be the API token you generated (as explained in the License section above)

### Request Body 
Elasticsearch Search API Body as documented  in [Elaticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.x/search.html), with the following restrictions:
* We support the following top-level elements in the Elasticsearch Search API:  `query`, `from`, `size`, `sort`, `_source`, `post_filter`, `aggs`, `aggregations`
* When using the `query_string` element, you are not allowed to set its field named `allow_leading_wildcard` to true
* When using the `wildcard` element, you are not allowed to have its value start with `*` or `?`
* Your query will only run on data you sent today and yesterday. You can still specify a filter on the timestamp field to allow smaller time frame.
* When specifying the `size` element for aggregations, the value cannot exceed 1000.
* You can not nest 2 or more bucket aggregations of the following types: `date_histogram`, `geohash_grid`, `histogram`, `ip_ranges`, `significant_terms` and `terms`
* You can not sort on the `message` field
* You can not aggregate on the `message` field
* You can not use the aggregation type `significant_terms`
* You can not use the `fuzzy_max_expansions` or `max_expansions` elements inside the `query` element
* You can not use the `max_determinized_states` element inside the `query` element

### Response

----------------------
| Status code | Body    | Meaning |
| ----------- | ------- | ---- | 
| 200         | The result of the query, as documented in Elasticsearch Search API (link above) | Query succeeded |
| 400         | "Found JSON elements which are not allowed [...]" | See Limits above for list of allowed elements in Query |
| 400         | "[...] query_string.allow_leading_wildcard not allowed to be true" | |
| 400         | "[...] 'wildcard' queries are not allowed to start with '*' or '?'" | |
| 400         | "[...] contains a wildcard search with leading wildcard which is not allowed [...]" | |
| 400         | "Query has failed. Elasticsearch Response: [...]" | Elasticsearch failed running this search request. See the Elasticsearch response to understand what went wrong. Most likely the syntax of the search request does not conform with the search API DSL - see link above for full documentation |
| 400         | "Query syntax (in 'query' field) is invalid: [...]" | |
| 400         | "This search can't be executed: [...]. Please contact customer support for more details" | Something unexpected in the query prevented him from running. Contact our customer support for help | 
| 403         |  | Your account is not allowed to use Search API. Please contact support/sales to enable this feature for you|
| 429         |  | Exceeded your account rate limit of searches |
| 429         | "Your account is allowed to run up to __ concurrent queries via the API" | Your account is allowed to run up to X concurrent queries via the Search API. This error means you've exceeded this limit |
| 503         |  | Our service is temporarily unavailable. You should try again when that happens. |
| All other codes | A message that explain the specific error | Query failures |
----------------------


## Example

Create a file named `query.json` and place the following query in it:
```json
{
	"size": 0,
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
	},
	"aggs": {
		"byType": {
			"terms": {
				"field": "type",
				"size": 5
			}
		}
	}
}
```
This query returns the count of documents per each value of the field `type`, in the last 5 minutes

Run the following command:
```bash
curl -XPOST 'https://api.logz.io/v1/search'  --header "X-USER-TOKEN: your-api-access-token" --header "Content-Type: application/json" --data-binary @query.json
```

Make sure to replace `your-api-access-token` with the token you created as instructed in License section

Example result:

```json
{
  "hits": {
    "total": 339604,
    "max_score": 0,
    "hits": []
  },
  "aggregations": {
    "byType": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 44879,
      "buckets": [
        {
          "key": "web-app",
          "doc_count": 163690
        },
        {
          "key": "core-service",
          "doc_count": 64893
        }      
      ]
    }
  }
}
```