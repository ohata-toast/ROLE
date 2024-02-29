## Application Service > ROLE > API Guide


> To check permissions using the ROLE service, call the RESTful API or use the Client SDK.

## AppKey & SecretKey

AppKey and Secret Key are required to use RESTful API and Client SDK.
You can check the issued key information by clicking the **URL & Appkey** button on the top right of the [CONSOLE].

![[Figure 1] Check AppKey & SecretKey](http://static.toastoven.net/prod_role/role_60.png)
<center>[Figure 1] Check AppKey &amp; SecretKey</center>

## RESTful API Guide

### Common Response Body

For all API requests, the HTTP response code is 200.
For detailed response results, see the headers in the response body.

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|Key|	Type|	Description|
|---|---|---|
|header|	Object|	[Response Header]|
|header.isSuccessful|	boolean|	Successful or not|
|header.resultCode|	int|	Response code. Returns 0 on success or an error code on failure.|
|header.resultMessage|	String|	Response message. Returns "SUCCESS" on success or an error message on failure.|

### 1. User

#### 1.1. Register a User

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"users": [
		{
			"description": "",
			"relations": [
				{
					"roleId": "",
					"scopeId": ""
				}
			],
			"userId": ""
		}
	]
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|users|	List|	Yes|	User Registration Information|
|users[0].userId|	String|	Yes|	User ID <br/> You can register up to 48 characters. <br/> -_@. You can use special characters, and the ID must start and end with a letter and a number.|
|users[0].description|	String|	Yes|	User Description <br/> You can register up to 128 characters.|
|users[0].relations|	List|	No|	User - Role relationship list|
|users[0].relations[0].roleId|	String|	Yes|	Role ID|
|users[0].relations[0].scopeId|	String|	Yes|	Scope ID|
|users[0].relations[0].validStartDate|	Date|	No| 	Validity period start date for the role granted to the user (end of support after 2024-01-23) |
|users[0].relations[0].validEndDate|	Date|	No| 	End of validity date for the role granted to the user (end of support after 2024-01-23) |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"errors" : [
		{
			"code": 0,
			"message": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|errors|	List|	Returns a list of errors, or an empty list if no errors occurred.|
|errors[0].code|	int|	Error Code|
|errors[0].message|	String|	Error Message|

#### 1.2. Get User

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE]|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"user": {
		"appKey": "",
		"description": "",
		"regYmdt": "2019-11-01T00:00:00.000+0000",
		"userId": ""
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|user|	Object|	User Information|
|user.appKey|	String|	AppKey|
|user.userId|	String|	User ID|
|user.description|	String|	User Description|
|user.regYmdt|	Timestamp|	Registration Date|

#### 1.3. Get Users

If you pass in a Scope ID and Role ID, it will return only the Users with that role.
If you set includeRelation to true, it will also include and return Users with roles that are related to the Role ID.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Query Parameter]**

|Key|	Value| Required |
|---|---|---|
|scopeId|	Scope ID| No |
|roleId|	Role ID| No |
|includeRelation| true or false| No |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode" : 0,
		"resultMessage" : "Success."
	},
    "users" :
    [
        {
            "description" : "",
            "regYmdt" : "2019-11-01T00:00:00.000+0000",
            "relations" :
            [
                {
                    "roleId" : "",
                    "scopeId" : ""
                }
            ],
            "userId" : ""
        }
    ]
}
```

|Key|	Type|	Description|
|---|---|---|
|users|	List|	List of user information|
|users[0].appKey|	String|	AppKey|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User Description|
|users[0].regYmdt|	Timestamp|	Registration Date|
|users[0].relations | List | List of relationships assigned to User |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | Validity period start date for the role granted to the user (end of support after 2024-01-23)|
|users[0].relations[0].validEndDate | Date | End of validity date for the role granted to the user (end of support after 2024-01-23)|

#### 1.4. Get Bulk Users

API to get user information all at once

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"usersIds": [
		""
	]
}
```

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode" : 0,
		"resultMessage" : "Success."
	},
    "users" :
    [
        {
            "description" : "",
            "regYmdt" : "2019-11-01T00:00:00.000+0000",
            "relations" :
            [
                {
                    "userId" : "",
                    "roleId" : "",
                    "scopeId" : ""
                }
            ],
            "userId" : ""
        }
    ]
}
```

|Key|	Type|	Description|
|---|---|---|
|users|	List|	List of user information|
|users[0].appKey|	String|	AppKey|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User Description|
|users[0].regYmdt|	Timestamp|	Registration Date|
|users[0].relations | List | List of relationships assigned to User |
|users[0].relations[0].userId | String | User ID |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | Validity period start date for the role granted to the user (end of support after 2024-01-23) |
|users[0].relations[0].validEndDate | Date | End of validity date for the role granted to the user (end of support after 2024-01-23) |


#### 1.5. Modify User Description

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	User Description|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 1.6. Delete User

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 1.7. Check Permissions

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/{userId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
	"resources": [
		{
			"operationId": "",
			"resourceId": "",
			"resourcePath": "",
			"scopeId": ""
		}
	]
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|Resources|	List|	Yes|	Resource list to check permissions|
|resources[0].operationId|	String|	Yes|	Operation ID|
|resources[0].resourceId|	String|	No|	You must include one of the following values: Resource ID, Resource ID, and Path.|
|resources[0].resourcePath|	String|	No|	You must include one of the following values: Resource Path, Resource ID, and Path.|
|resources[0].scopeId|	String|	Yes|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"authorizations": [
		{
			"operationId": "",
			"permission": true,
			"resourceId": "",
			"resourcePath": "",
			"scopeId": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|authorizations|	List|	List of permission check results|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].permission|	boolean|	Permission check results|
|authorizations[0].resourceId|	String|	Resource ID|
|authorizations[0].resourcePath|	String|	Resource Path|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.8. Check Role Permissions

Returns whether the User has been granted a role. Also includes roles based on associations.

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/{userId}/authorizations/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
	"roles": [
		{
			"roleId": "",
			"scopeId": ""
		}
	]
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roles|	List|	Yes|	List of roles to check permissions|
|roles[0].roleId|	String|	Yes|	Role ID|
|roles[0].scopeId|	String|	Yes|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"authorizations": [
		{
			"permission": true,
			"roleId": "",
			"scopeId": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|authorizations|	List|	List of permission check results|
|authorizations[0].permission|	boolean|	Permission check results|
|authorizations[0].roleId|	String|	Role ID|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.9. Get Role assigned to User

Returns only directly granted roles. It does not return roles that are related to a role.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|
|userId|	User ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"relations": [
		{
			"appKey": "",
			"roleId": "",
			"scopeId": "",
			"userId": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|relations|	List|	User - Role relationship list|
|relations[0].appKey|	String|	Operation ID|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|
|relations[0].validStartDate|	Date|Validity period start date for the role granted to the user (end of support after 2024-01-23)|
|relations[0].validEndDate|	Date|End of validity date for the role granted to the user (end of support after 2024-01-23)|

#### 1.10. Give User a Role

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
	"roleId": "",
	"scopeId": "",
	"createUserIfNotExist": false
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleId|	String|	Yes|	Role ID|
|scopeId|	String|	Yes|	Scope ID|
|createUserIfNotExist| Boolean| No| Whether to create a User when no User exists|
|validStartDate|	Date|	No|	Validity period start date for the role granted to the user (end of support after 2024-01-23)|
|validEndDate|	Date|	No|	End of validity date for the role granted to the user (end of support after 2024-01-23) |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 1.11. Delete the Role assigned to User

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Query Parameter]**

|Key|	Value|	Required|
|---|---|---|
|roleId|	Role ID|	Yes|
|scopeId|	Scope ID|	Yes|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 1.12. Delete an existing role for a user and give them a new role

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
	"relations": [
		{
			"roleId": "",
			"scopeId": ""
		}
	]
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|relations|	List|	No|	User - Role relationship list|
|relations[0].roleId|	String|	Yes|	Role ID|
|relations[0].scopeId|	String|	Yes|	Scope ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
#### 1.13. Set an expiration date for a role granted to a user

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles/valid-period|


**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|userId|	User ID|

**[Request Body]**

```json
{
    "roleId" : "",
    "scopeId" : ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleId|	String|	Yes|	Role ID|
|scopeId|	String|	Yes|	Scope ID|
|validStartDate|	Date|	No|	Validity period start date for the role granted to the user (end of support after 2024-01-23)|
|validEndDate|	Date|	No|	End of validity date for the role granted to the user (end of support after 2024-01-23)|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 2. Scope

#### 2.1. Register a Scope

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"description": "",
	"scopeId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|scopeId|	String|	Yes|	Scope ID <br/> You can register up to 32 characters. </br> You can use the -_ special character, and the ID must start and end with a letter and a number. |
|description|	String|	Yes|	Scope description <br/> You can register up to 128 characters.|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 2.2. Get Scope

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|scopeId|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"scope": {
		"appKey": "",
		"description": "",
		"scopeId": ""
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|scope|	Object|	Scope Information|
|scope.appKey|	String|	AppKey|
|scope.scopeId|	String|	Scope ID|
|scope.description|	String|	Scope description|

#### 2.3. Edit Scope Description

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|scopeId|	Scope ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Scope description|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 2.4. Delete Scope

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|scopeId|	Scope ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 2.5. Get relationships associated with Scope

Gets associations related to a Scope ID.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scope/{scopeId}/relations

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|scopeId|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"relations": [
		{
			"appKey": "",
			"roleId": "",
			"scopeId": "",
			"userId": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|relations|	List|	User - Role relationship list|
|relations[0].appKey|	String|	Operation ID|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|


#### 2.6. Get Scope List

Gets lists in the form of pages.
Entering 1 for page and 10 for itemsPerPage will retrieve the first 10 lists.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Query Parameter]**

|Key|	Value|	Required|	Description|
|---|---|---|---|
|scopeId|	Scope ID|	No|	|
|description|	|	No|	Description|
|page|  |	No|	Start with 1 as the page number you want to search|
|itemsPerPage|  |	No|	Number of records in the scopes for which you want results|

**[Response Body]**

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success"
  },
  "scopes": [
    {
      "description": "",
      "scopeId": ""
    }
  ],
  "totalItems": 0
}
```

|Key|	Type|	Description|
|---|---|---|
|scopes|	List|	Scope Information|
|scopes[0].description|	String|	Scope description|
|scopes[0].scopeId|	String|	Scope ID|
|totalItems|	int|	Total number of scopes|

### 3. Role

#### 3.1. Register a Role

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"description": "",
	"roleId": "",
	"roleName" :  "",
	"roleGroup" :  "",
    "exposureOrder": 0
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleId|	String|	Yes| 	Role ID <br/> You can register up to 128 characters. <br/> You can use the special characters `-`, `_`, `.`, `:`, and the ID must start and end with a letter and a number. |
|description|	String|	Yes|	Role description <br/> You can register up to 128 characters.|
|roleName|	String|	No|	Role name <br/> You can give it a meaningful name. It can be up to 128 characters long.|
|roleGroup|	String|	No|	Role Group <br/> Roles can be grouped together for administrative purposes. You can register up to 128 characters.|
|exposureOrder|	int|	No|	Exposure order <br/> Numeric only. Default value 0|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.2. Get Role

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|
|roleName| Role name|
|roleGroup| Role Group|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"role": {
		"appKey": "",
		"description": "",
		"roleId": "",
		"roleName": "",
		"roleGroup": "",
		"exposureOrder": 0,
		"regDateTime": "",
		"roleTags": [ {"roleTagId": ""}]
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|role|	Object|	About roles|
|role.appKey|	String|	AppKey|
|role.roleId|	String|	Role ID|
|role.description|	String|	Role description|
|role.roleName|	String|	Role name|
|role.roleGroup|	String|	Role group name|
|role.exposureOrder|	int|	Exposure order|
|role.regDateTime|	String|	At enrollment|
|role.roleTags|	Object|	About Tags |
|role.roleTags.roleTagId|	String|	Tag ID|

#### 3.3. Edit Role information

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|

**[Request Body]**

```json
{
  "description": "",
  "roleName": "",
  "roleGroup": "",
  "exposureOrder": 0
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	User Description|
|roleName|	String|	No|	Role name <br/> You can give it a meaningful name. It can be up to 128 characters long.|
|roleGroup|	String|	No|	Role Group <br/> Roles can be grouped together for administrative purposes. You can register up to 128 characters.|
|exposureOrder|	int | No | Exposure order|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.4. Delete a Role

**[Method, URL]**

|Method|	URI|
|---|---|
|Delete|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|


**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.5. Set up Role associations

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"relatedRoleId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|relatedRoleId|	String|	Yes|	Role ID to set the association to|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.6. Delete a Role association

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations/{relatedRoleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|
|relatedRoleId|	Associated Role ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.7. Assign User to Role


**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"createUserIfNotExist": false,
	"users": [
		{
			"scopeId": "",
			"userId": ""
		}
	]
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|createUserIfNotExist| Boolean| No| Whether to create a User when no User exists|
|users|	List|	Yes|	Users list|
|users[0].scopeId|	String|	No|	Scope ID, default ALL if not present|
|users[0].userId|	String|	Yes|	User ID|
|users[0].validStartDate|	Date|	No|	Validity period start date for the role granted to the user (end of support after 2024-01-23)|
|users[0].validEndDate|	Date|	No|	End of validity date for the role granted to the user (end of support after 2024-01-23)|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.8. Get Roles

Gets lists in the form of pages.
Entering 1 for page and 10 for itemsPerPage will retrieve the first 10 lists.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Query Parameter]**

|Key|	Value| Required |
|---|---|---|
|roleId|	Role ID| No |
|description|	Description|	No|
|roleName|	Role name|	No|
|roleGroup|	Role Group name|	No|
|roleTagIds| Tag Id Condition (;for OR, ,for AND)|	No|
|page|  Start with 1 as the page number you want to search|	No|
|itemsPerPage|  Number of records in the scopes for which you want results|	No|

Through roleTagIds, you can search for the Tag set in the Role as an AND or OR condition when searching.
For example, if you want to search for a role that has A and B tags in the role, you can create a condition as A;B,
If you want to search for only one of the A or B Tags, you can create a condition with A,B.
You can also create conditions such as (A;B),C.

**[Response Body]**

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success."
  },
  "roles": [
    {
      "description": "",
      "relatedRoleIds": [
        {}
      ],
      "roleId": "",
      "roleName": "",
      "roleGroup": "",
      "exposureOrder": 0,
      "regDateTime": "",
      "roleTags": [{ "roleTagId": ""}]
    }
  ],
  "totalItems": 0
}
```

|Key|	Type|	Description|
|---|---|---|
|roles|	List|	About roles|
|roles[0].description|	String|	Role description|
|roles[0].relatedRoleIds|	List|	Associated RoleIds|
|roles[0].roleId|	String|	Role ID|
|roles[0].roleName|	String|	Role name|
|roles[0].roleGroup|	String|	Role group name|
|roles[0].exposureOrder|	int|	Exposure order|
|roles[0].regDateTime|	String|	At enrollment|
|roles[0].roleTags|	Object|	About Tags |
|roles[0].roleTags.roleTagId|	String|	Tag ID|
|totalItems|	int|	Total number of roles|


#### 3.9. Create a Role Tag

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"roleTagId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleTagId|	String|	Yes|	Tag ID to assign|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


#### 3.10. Delete Role Tag

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags/{roleTagId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|
|roleTagId|	Tag ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 3.11. Get Role Tag

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|roleId|	Role ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
    "roleTags" : [{
        "roleTagId" : ""
    }]
}
```
|Key|	Type|	Description|
|---|---|---|
|roleTags|	List|	About Tags|
|roleTags[0].roleTagId|	String|	Tag ID|

### 4. Resource

#### 4.1. Create a Resource

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"description": "",
	"metadata": "",
	"name": "",
	"path": "",
	"priority": 0,
	"resourceId": "",
	"uiPath": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|resourceId|	String|	Yes|	Resource ID <br/> You can register up to 32 characters. <br/> You can use the -_ special character, and the ID must start and end with a letter and a number.|
|name|	String|	No|	Not required.|
|path|	String|	Yes|	Resource path <br/> You can register up to 1024 characters. <br/> The Resource path must consist of a combination of '/'. <br/> The exception is {}, which can represent a path variable.|
|description|	String|	Yes|	Resource Description <br/> You can register up to 128 characters.|
|priority|	smallint|	Yes|	Priorities seen on the same path <br/> The value can be between -32768 and 32767, with the lower values being more visible.|
|metadata|	String|	Yes|	Custom data <br/> You can register up to 65536 characters.|
|uiPath|	String|	Yes|	UI Path Path <br/> You can register up to 1024 characters. <br/> The UI Path path must consist of a combination of the Resource name and a '/'. |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 4.2. Get Resource Hierarchy

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/hierarchy|

**[Request Header]**

|Key|	Value|
|---|---|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Query Parameter]**

|Key|	Value|	Required|
|---|---|---|
|userId|	User ID|	No|
|roleId|	Role ID|	No|
|scopeId|	Scope ID|	No|
|operationId|	Operation ID|	No|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"resources": [
		{
			"description": "",
			"metadata": "",
			"name": "",
			"path": "",
			"priority": 0,
			"resourceId": "",
			"resources": []
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|Resources|	List|	Resource list|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].description|	String|	Resource Description|
|resources[0].name|	String|	Resource name|
|resources[0].path|	String|	Resource path|
|resources[0].priority|	smallint|	Priority|
|resources[0].metadata|	String|	Custom data|
|resources[0].resources|	List|	Resource list|

#### 4.3. Get Resource

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|resourceId|	Resource ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"resource": {
		"appKey": "",
		"description": "",
		"metadata": "",
		"name": "",
		"path": "",
		"priority": 0,
		"resourceId": ""
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|resource|	Object|	Resource information|
|resource.appKey|	String|	AppKey|
|resource.resourceId|	String|	Resource ID|
|resource.description|	String|	Resource Description|
|resource.name|	String|	Resource name|
|resource.path|	String|	Resource path|
|resource.priority|	smallint|	Priority|
|resource.metadata|	String|	Custom data|

#### 4.4. Modify Resource

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|resourceId|	Resource ID|

**[Request Body]**

```json
{
	"description": "",
	"metadata": "",
	"name": "",
	"path": "",
	"priority": 0
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|name|	String|	No| Not required. |
|path|	String|	Yes| 	Resource path <br/> You can register up to 1024 characters. <br/> The Resource path must consist of a combination of '/'. <br/> The exception is {}, which can represent a path variable.|
|description|	String|	Yes|	Resource Description <br/> You can register up to 128 characters.|
|priority|	smallint|	Yes|	Priorities seen on the same path <br/> The value can be between -32768 and 32767, with the lower values being more visible.|
|metadata|	String|	Yes|	Custom data <br/> You can register up to 65536 characters.|
|uiPath|	String|	Yes| 	UI Path Path <br/> You can register up to 1024 characters. <br/> The UI Path path must consist of a combination of the Resource name and a '/'. |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 4.5. Delete a Resource

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|resourceId|	Resource ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 4.6. Get permissions associated with a Resource

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|resourceId|	Resource ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"authorizations": [
		{
			"operationId": "",
			"roleId": ""
		}
	]
}
```

|Key|	Type|	Description|
|---|---|---|
|authorizations|	List|	List of permissions information|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].roleId|	String|	Role ID|

#### 4.7. Add permissions to the Resource.

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|resourceId|	Resource ID|

**[Request Body]**

```json
{
	"operationId": "",
  	"roleId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|operationId|	String|	Yes|	Operation ID|
|scopeId|	String|	No|	Scope ID, default ALL if not present|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 4.8. Get Resources

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Query Parameter]**

|Key|	Value|	Required|	Description |
|---|---|---|---|
|userId|	|	No|	|
|roleId|	|	No|	|
|operationId|  |	No|	|

**[Response Body]**

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success."
  },
  "resources": [
    {
      "description": "",
      "metadata": "",
      "name": "",
      "path": "",
      "priority": 0,
      "resourceId": "",
      "uiPath": ""
    }
  ]
}
```

|Key|	Type|	Description|
|---|---|---|
|Resources|	List|	Resource information|
|resources[0].description|	String|	Resource Description|
|resources[0].metadata|	String|	Custom data|
|resources[0].name|	String|	Resource name|
|resources[0].path|	String|	Resource path|
|resources[0].priority|	smallint|	Priority|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].uiPath|	String|	uiPath|

### 5. Operation

#### 5.1. Register an Operation

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Request Body]**

```json
{
	"description": "",
	"operationId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|operationId|	String|	Yes|	Operation ID <br/> You can register up to 32 characters. <br/> You can use the -_ special character, and the ID must start and end with a letter and a number.|
|description|	String|	Yes|	Operation description <br/> You can register up to 128 characters.|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 5.2. Get Operation

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|operationId|	Operation ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	},
	"operation": {
		"appKey": "",
		"description": "",
		"operationId": ""
	}
}
```

|Key|	Type|	Description|
|---|---|---|
|operation|	Object|	Operation information|
|operation.appKey|	String|	AppKey|
|operation.operationId|	String|	Operation ID|
|operation.description|	String|	Operation description|

#### 5.3. Edit the Operation description

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|operationId|	Operation ID|

**[Request Body]**


```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Operation description|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 5.4. Delete an Operation

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|
|operationId|	Operation ID|

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


#### 5.5. Get Operations

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	SecretKey issued by [CONSOLE].|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	AppKey issued by [CONSOLE]|

**[Response Body]**

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success."
  },
  "operations": [
    {
      "appKey": "",
      "description": "",
      "operationId": ""
    }
  ]
}
```

|Key|	Type|	Description|
|---|---|---|
|operations|	List|	Operation information|
|operations[0].appKey|	String|	AppKey|
|operations[0].description|	String|	Operation description|
|operations[0].operationId|	String|	Operation ID|
