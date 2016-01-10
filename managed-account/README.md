# Managed Accounts API

## General
Managed Accounts are accounts created and managed within the scope of another account. For example, a customer of Logz.io which is a service provider by itself, may wish to create and maintain Managed Accounts for its service receivers. 

## Limitations
Using the API requires a special license from Logz.io, and an API token.

The number of Managed Accounts created from within another account is controlled and limited by Logz.io.

## Create Managed Account
### Example:
```
$ curl -XPOST 'https://api.logz.io/v1/managed-account'  
  --header "X-USER-TOKEN : your-api-access-token" 
  -d '{ 
    "email" : "john@doe.com",
    "accountName" : "My awesome company",
    "maxDailyGB" : 1,
    "retentionDays" : 7
  }'
```
### Request Header:
- X-USER-TOKEN : contains the token which was provided by Logz.io in order to access the API.

### Request Body, in JSON Format:
- email : the email of the user to associate with the new managed account. Note that the user role in the new account is admin. The new user should be an existing user of the master account.
- fullName: the name of the user.
- company: the company name of the new account.
- maxDailyGB: float, the maximum daily quota, in GigaBytes.
- retentionDays: the number of days to keep the data within Logz.io.

### Response Body, in JSON Format, in Case of Successful Response (200 OK):
- accountId : the id of the new account which was created.
- accountName : the name of the new account.
- loggingToken : the token which should be used in order to ship logs to the new account.

### Errors


> {
> "code": 403,
> "message": "Max Managed Accounts limit reached on this account"
> }

> {
>     "code": 403,
>     "message": "Managed Account max-daily-gb(122.0) is above the allowed-limit(1.0)"
> }

> {
>     "code": 403,
>     "message": "Managed Account retentionDays(110) is above the allowed-limit(14)"
> }

> {
>     "code": 403,
>     "message": "Email must belong to an existing user of the managing account"
> }


## List Managed Accounts
### Example:
```
$ curl -XGET ‘https://api.logz.io/v1/managed-account’
	--header "X-USER-TOKEN : your-api-access-token"
```

### Request Header:
- X-USER-TOKEN : contains the token which was provided by Logz.io in order to access the API.

### Request Body : No body is Required.

### Response Body:
In Case of Successful Response (200 OK) a json array of managed accounts, for example:

```
 [ {
    "accountId":777, 
    "accountName":"Managed Account Name",
    "loggingToken":"TOKEN-To-Ship-Logs",
    "adminEmails":["john@doe.com","bob@doe.com"]
    "maxDailyGB":0.5,
    "retentionDays":7 
    } , ....]
```



## Delete Managed Account
### Example:
```
$ curl -XDELETE ‘https://api.logz.io/v1/managed-account/{id}’
	--header "X-USER-TOKEN : your-api-access-token"
```

### Request Path Parameter:
- id : The id of the Managed Account to delete

### Request Header:
- X-USER-TOKEN : contains the token which was provided by Logz.io in order to access the API.

### Request Body : 
No body is required.

### Response Body: 
No body, 200 OK in case the account was successfully deleted.
