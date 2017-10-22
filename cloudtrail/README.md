# CloudTrail API

## General
The CloudTrail API allows creating, updating, reading and deleting.
During a create or update operation the CloudTrail credentials are validated.

## Limitations
Using the API requires a special license from Logz.io, and an API token which can be generated here: https://app.logz.io/#/dashboard/settings/shared-tokens 

The number of queries executed is controlled and limited by Logz.io.


**Get Account CloudTrails**
----
  Returns a json list of account defined CloudTrails.

* **URL**

  /log-shipping/cloudtrails

* **Method:**

  `GET`

* **Success Response:**

  * **Code:** 200 OK <br />
    **Content:** `[{
    	"id": 15, 
    	"accountId": 2401,
    	"accessKey": "s3AccessKeyHere",
    	"bucket": "thebucketname",
    	"prefix": "AWSLogs/7364988021587/myprefix"
    }, { "id": 16, "accountId": 2401, "accessKey": "s3AccessKeyHere", "bucket": "anotherbucket", "prefix": "AWSLogs/68117300854/anotherprefix"}]`


**Get Specific CloudTrail**
----
  Returns a specfic CloudTrail settings by id.

* **URL**

  /log-shipping/cloudtrails/:id

* **Method:**

  `GET`

*  **URL Params**
 
   `id=[integer]` - settings id to get

* **Success Response:**

  * **Code:** 200 OK <br />
    **Content:** `{
    	"id": 15, 
    	"accountId": 2401,
    	"accessKey": "s3AccessKeyHere",
    	"bucket": "thebucketname",
    	"prefix": "AWSLogs/7364988021587/myprefix"
    ]`

**Create CloudTrail**
----
  Create new CloudTrail settings.

* **URL**

  /log-shipping/cloudtrails

* **Method:**

  `POST`

*  **Data Params**

	**Required:** <br />
   `bucket=[string]` - cloudtrail bucket path . <br />
   		example - "LogzIoBucket" <br />
   `prefix=[string]` - prefix inside the bucket. <br />
   		example - "AWSLogz/5819239577123/CloudTrail" <br />
   `accessKey=[string]` - S3 access key for accessing CloudTrail. <br />
   `secretKey=[string]` - S3 secret key for accessing CloudTrail. <br />

* **Success Response:**

  * **Code:** 200 CREATED <br />
    **Content:** ID of the created cloudtrail:
    	`{"id": 21}`

**Update CloudTrail**
----
  Create a new CloudTrail setting.

* **URL**

  /log-shipping/cloudtrails/:id

* **Method:**

  `PUT`

*  **URL Params**
 
   `id=[integer]` - settings id to update

*  **Data Params** 

   `bucket=[string]` - cloudtrail bucket path . <br />
   		example - "LogzIoBucket" <br />
   `prefix=[string]` - prefix inside the bucket. <br />
   		example - "AWSLogz/5819239577123/CloudTrail" <br />
   `accessKey=[string]` - S3 access key for accessing CloudTrail. <br />
   `secretKey=[string]` - S3 secret key for accessing CloudTrail. <br />
   `active=[boolean]` - change CloudTrail to be active/disabled. <br />

* **Success Response:**

  * **Code:** 200 OK <br />
    **Content:** `{"message": "CloudTrail deleted successfully."}`

**Delete CloudTrail**
----
  Delete a specfic CloudTrail settings by id.

* **URL**

  /log-shipping/cloudtrails/:id

* **Method:**

  `DELETE`

*  **URL Params**
 
   `id=[integer]` - settings id to delete

* **Success Response:**

  * **Code:** 200 OK <br />
    **Content:** `{"message": "CloudTrail deleted successfully."}`