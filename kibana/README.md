# Kibana Import/Export API

## General
The Kibana API allows import and export of kibana saved objects.

## License
Using the Logz.io API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens
In addition, using the Kibana API requires special license from logz.io.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Supported Actions**
---

***Export Kibana Objects***
----
  Returns a json list of saved objects and the kibana version.

* **URL**

  https://api.logz.io/v1/kibana/export

* **Http Method:**

  `POST`

* **Request:**
---
| Parameter|Description|Supported Values
|---|---|---|
| type| The object type to export | search/dashboard/visualization
---

* **Swagger Specification**
```json
{
  "/kibana/export": {
    "post": {
      "consumes": [],
      "produces": [
        "application/json"
      ],
      "parameters": [
        {
          "name": "body",
          "in": "body",
          "required": true,
          "schema": {
            "$ref": "#/definitions/KibanaExportRequest"
          }
        }
      ],
      "responses": {
        "200": {
          "description": "OK",
          "headers": {},
          "schema": {
            "$ref": "#/definitions/KibanaExportResponse"
          }
        }
      },
      "tags": [
        "export"
      ]
    }
  },
  "KibanaExportRequest": {
    "properties": {
      "type": {
        "type": "string",
        "enum": [
          "dashboard",
          "search",
          "visualization"
        ]
      }
    }
  },
  "KibanaExportResponse": {
    "properties": {
      "hits": {
        "type": "array",
        "items": {
          "$ref": "#/definitions/Object"
        }
      },
      "kibanaVersion": {
        "type": "string"
      }
    }
  }
}
```
***example***
```json
{
  "type": "search"
}
```

* **Success Response:**

**Code:** 200 OK
  
**Content:** 
```json
{
  "kibanaVersion": "4.0.0-beta3",
  "hits": [
    {
      "_index": "logzioCustomerKibanaIndex",
      "_type": "search",
      "_id": "c245e530-fc60-11e7-b120-5320b1759c57",
      "_score": 1,
      "_source": {
        "title": "example-saved-query",
        "description": "",
        "hits": 0,
        "columns": [
          "message"
        ],
        "sort": [
          "@timestamp",
          "desc"
        ],
        "version": 1,
        "kibanaSavedObjectMeta": {
          "searchSourceJSON": "{\"index\":\"[logzioCustomerIndex]YYMMDD\",\"highlightAll\":true,\"version\":true,\"query\":{\"query_string\":{\"query\":\"example-saved-query\"}},\"filter\":[]}"
        },
        "_createdBy": {
          "userId": 1,
          "fullName": "testadmin@logz.io",
          "username": "testadmin@logz.io"
        },
        "_createdAt": 1516287816456,
        "_updatedBy": {
          "userId": 1,
          "fullName": "testadmin@logz.io",
          "username": "testadmin@logz.io"
        },
        "_updatedAt": 1516287816456
      }
    }
  ]
}

```

***Import Kibana Objects***
----
  Imports a json list of kibana saved object.

* **URL**

  https://api.logz.io/v1/kibana/import

* **Http Method:**

  `POST`

* **Request:**

---
| Parameter|Description|
|---|---|
| kibanaVersion| The version of the kibana where the saved objects was generated from. User can only import files that was generated from the same kibana version|
| override| true will override existing key with new data|
| hits| array of saved objects|
---

***example***
```json
{
  "kibanaVersion": "4.0.0-beta3",
  "override": true,
  "hits": [
    {
      "_index": "logzioCustomerKibanaIndex",
      "_type": "search",
      "_id": "c245e530-fc60-11e7-b120-5320b1759c57",
      "_score": 1,
      "_source": {
        "title": "example-saved-query",
        "description": "",
        "hits": 0,
        "columns": [
          "message"
        ],
        "sort": [
          "@timestamp",
          "desc"
        ],
        "version": 1,
        "kibanaSavedObjectMeta": {
          "searchSourceJSON": "{\"index\":\"[logzioCustomerIndex]YYMMDD\",\"highlightAll\":true,\"version\":true,\"query\":{\"query_string\":{\"query\":\"example-saved-query\"}},\"filter\":[]}"
        },
        "_createdBy": {
          "userId": 1,
          "fullName": "testadmin@logz.io",
          "username": "testadmin@logz.io"
        },
        "_createdAt": 1516287816456,
        "_updatedBy": {
          "userId": 1,
          "fullName": "testadmin@logz.io",
          "username": "testadmin@logz.io"
        },
        "_updatedAt": 1516287816456
      }
    }
  ]
}
```

* **Swagger Specfication**
```json
{
  "/kibana/import": {
    "post": {
      "consumes": [],
      "produces": [
        "application/json"
      ],
      "parameters": [
        {
          "name": "body",
          "in": "body",
          "required": true,
          "schema": {
            "$ref": "#/definitions/KibanaImportRequest"
          }
        }
      ],
      "responses": {
        "200": {
          "description": "OK",
          "headers": {},
          "schema": {
            "$ref": "#/definitions/KibanaImportResponse"
          }
        }
      },
      "tags": [
        "import"
      ]
    }
  },
  "KibanaImportRequest": {
    "properties": {
      "hits": {
        "type": "array",
        "items": {
          "$ref": "#/definitions/Map"
        }
      },
      "kibanaVersion": {
        "type": "string"
      },
      "override": {
        "type": "boolean"
      }
    }
  },
  "KibanaImportResponse": {
    "properties": {
      "created": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "failed": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "ignored": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "updated": {
        "type": "array",
        "items": {
          "type": "string"
        }
      }
    }
  }
}
```

* **Success Response**

**Code:** 200 OK

---
| Parameter|Description|
|---|---|
| created| array of created ids|
| updated| array of updated ids|
| ignored| array of ignored ids|
| failed| array of failed ids|
---

**Content:** 
  
```json
{
  "created": [],
  "updated": [],
  "ignored": [
    "c245e530-fc60-11e7-b120-5320b1759c57"
  ],
  "failed": []
}
```

