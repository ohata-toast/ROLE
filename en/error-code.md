## Application Service > ROLE > Error Code
Response Body contains "header" information by default. 
If API call fails, isSuccessful becomes false and an error code is displayed in resultCode.

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
The resultCode is defined as follows.

| isSuccessful | resultCode | Description |
|--|--|--|
| TRUE | 0 | Successful |
| FALSE | -2 | Failed due to server issue. Need to check with person-in-charge |
| FALSE | 400 | API call failed due to invalid factor |
| FALSE | Appkey is invalid | invalid appkey, secretkey |
| FALSE | 403 | Failed due to call from unauthorized IP |
| FALSE | 404 | Check API URL and HTTP methods |
| FALSE | Exceeded server connection time | Failed due to server issue. Need to check with person-in-charge |
| FALSE | 1000 | Invalid Request Parameters |
| FALSE | 1002 | Request Parameters that failed validation |
| FALSE | 1003 | Failed to return response due to server issue |
| FALSE | 1004 | Failed due to server issue |
| FALSE | 1005 | Failed due to server issue |
| FALSE | 1200 | Failed due to server issue |
| FALSE | 1201 | Failed due to server issue |
| FALSE | 4003 | Invalid appkey |
| FALSE | 10000 | Invalid resource path |
| FALSE | 10001 | Invalid resource name |
| FALSE | 10002 | Invalid resource ID |
| FALSE | 10003 | Duplicate resource path exists |
| FALSE | 10004 | Duplicate resource exists |
| FALSE | 10005 | Resource not found |
| FALSE | 10006 | Too many resources registered |
| FALSE | 10007 | Duplicate resource UI Path |
| FALSE | 10008 | Invalid resource UI Path name |
| FALSE | 11000 | Duplicate roles exist |
| FALSE | 11001 | Role not found |
| FALSE | 11002 | Invalid Role ID |
| FALSE | 11003 | Role relationship already exists |
| FALSE | 11004 | Role relationship not found |
| FALSE | 11005 | Role and resource relationship already exists |
| FALSE | 11007 | Role relationship unspecified role specified |
| FALSE | 11008 | Role Relationship Setting Parameters Error |
| FALSE | 11010 | Unable to set to deny  |
| FALSE | 12000 | Operation not found |
| FALSE | 12001 | Operation already exists |
| FALSE | 12002 | Invalid Operation ID |
| FALSE | 13000 | Range ID already exists |
| FALSE | 13001 | Range ID not found |
| FALSE | 13002 | Do not allow changes to ALL range |
| FALSE | 13003 | Invalid range ID |
| FALSE | 14000 | User ID already exists |
| FALSE | 14001 | User ID not found |
| FALSE | 14002 | User role relationship already exists |
| FALSE | 14003 | User role relationship not found |
| FALSE | 14004 | Invalid User ID |
| FALSE | 14005 | Invalid period setting |
| FALSE | 15000 | Excel file processing failed |
| FALSE | 15001 | Wrong Excel file format |
| FALSE | 20000 | Condition attribute ID that already exists |
| FALSE | 20001 | Invalid condition attribute ID |
| FALSE | 20005 | Condition attribute ID already in use |
| FALSE | 21001 | Operator type of condition attribute value is not allowed |
| FALSE | 21002 | Invalid data type of condition attribute value |
| FALSE | 90000 | Failed due to systematic issue. Need to check person-in-charge |
