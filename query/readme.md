# Query API

## General
The Query API allows to run search queries on the data in your Logz.io account.

## Limitations
Using the API requires a special license from Logz.io, and an API token.

The number of queries executed is controlled and limited by Logz.io.

## Execute a Search Query

### Example:
```
$ curl -XPOST 'https://api.logz.io/v1/query'  
  --header "X-USER-TOKEN : your-api-access-token" 
  --header "Content-Type: application/json"
  -d '{ 
  	"queryString" : "Kong Foo",
    "timestamp" : {
        "gte" : 1443483053000,
        "lt" : 1444087853000
    }
  }'
```
### Request Header:
- X-USER-TOKEN : contains the token which was provided by Logz.io in order to access the API.

### Request Body, in JSON Format:

-------------------------------------
| Field Name  | Default Value |  Description |
| ------------- | ------------- | ------------- |
| queryString  | *  | The actual query to be parsed. See  [Query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/1.4/query-dsl-query-string-query.html#query-string-syntax). |
| timestamp.gte  | NOW - 15m | Greater-than or equal to. Timestamp in ms since the epoch (UNIX timestamp).  |
| timestamp.lt | NOW | Less-than. Timestamp in ms since the epoch.|
| from | 0 | Pagination of results can be done by using the from and size parameters. The from parameter defines the offset from the first result you want to fetch. |
| size | 200 | The size parameter allows you to configure the maximum amount of hits to be returned. |
-------------------------------------


### Response Body:
The body will contain the hits count and hits fetched from Elasticsearch.
Sample response:
```
{
  "hits":{
  "total" : 1,
  "hits" : [
    {
      "_index" : "logz",
      "_type" : "movies",
      "_id" : "1",
      "_source" : {
        "title" : "The Way of the Dragon",
        "year" : "1972",
        "Director" : "Bruce Lee"
      }
    }
  ]
}
```

### Errors


> {
>   "code": 403,
>   "message": "Query API is not permitted on this account"
> }

> {
>   "code": 400,
>   "message": "Query String syntax is invalid"
> }

> {
>   "code": 400,
>   "message": "Field:size is not valid. reason: must be between 0 and 500"
> }

