# Search API

## General
Search API gives you the ability to run queries via REST (i.e. HTTP). The query language is elasticsearch Search API DSL, with certain limitations. 

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here - https://app.logz.io/#/dashboard/account/tokens 

The number of queries executed is controlled and limited by Logz.io.

The number of results are limited to 10,000.

## Search Command

### URL
URL: https://api.logz.io/v1/search﻿
* Make sure to use https and not http, otherwise you will get 403 Forbidden.

### HTTP Headers
* "X-USER-TOKEN" -  The value should be the API token you generated (as explained in the License section above)

### Request Body 
Elasticsearch Search API Body as documented  in [Elaticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/search.html), with the following restrictions:
* The only top level elements we support in the Elasticsearch Search API are:  `query`, `from`, `size`, `sort`, `_source`, `post_filter`, `aggs`, `aggregations`
* When using `query_string` element, you are not allowed to set its field named `allow_leading_wildcard` to true
* When using `wildcard` element, you are not allowed to have its value start with `*` or `?`
* Your query will only run on data you sent from today and yesterday. You can still specify a filter on timestamp field to allow smaller time frame.
* When specifying `size` for aggregations, the value can not exceed 1000.
* You can not nest 2 or more bucket aggregations of the following types: `date_histogram`, `geohash_grid`, `histogram`, `ip_ranges`, `significant_terms` and `terms`

### Response

----------------------
| Status code | Meaning | Body |
| ----------- | ------- | ---- | 
| 200         | Query succeeded | The result of the query, as documented in Elasticsearch Search API (link above) |
| 503         |Our service is temporarily unavailable. You should try again when that happens. | |
| All other codes | Query failures | A message that explain the specific error |
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
			},
			"aggs": {
				"byLogLevel": {
					"terms": {
						"field": "loglevel",
						"size": 5
					}
				}
			}
		}
	}
}

```

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
          "doc_count": 163690,
          "byLogLevel": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "INFO",
                "doc_count": 163512
              },
              {
                "key": "WARN",
                "doc_count": 178
              }
            ]
          }
        },
        {
          "key": "core-service",
          "doc_count": 64893,
          "byLogLevel": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
              {
                "key": "TRACE",
                "doc_count": 940
              },
              {
                "key": "DEBUG",
                "doc_count": 20
              }
            ]
          }
        }      
      ]
    }
  }
}
```