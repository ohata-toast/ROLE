## Application Service > ROLE > API 가이드


> ROLE 서비스를 이용해 권한을 체크하기 위해서는
> RESTful API를 호출하거나, 클라이언트 SDK를 이용하여야 합니다.

## 앱키 & 비밀 키

RESTful API와 클라이언트 SDK를 사용하려면 앱키와 비밀 키가 필요합니다.
[CONSOLE] 우측 상단의 **URL & Appkey** 버튼을 클릭하여 발급 키 정보를 확인할 수 있습니다.

![[그림 1] 앱키 & 비밀 키 확인](http://static.toastoven.net/prod_role/role_60.png)
<center>[그림 1] 앱키 & 비밀 키 확인</center>

## RESTful API 가이드

### Common Response Body

모든 API 요청에 대해 HTTP 응답 코드는 200으로 응답합니다.
자세한 응답 결과는 Response Body의 header 항목을 참고합니다.

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

|Key|	Type| 	Description                            |
|---|---|-----------------------------------------|
|header|	Object| 	응답 헤더                                  |
|header.isSuccessful|	boolean| 	성공 여부                                  |
|header.resultCode|	int| 	응답 코드. 성공 시 0, 실패 시 오류 코드 반환           |
|header.resultMessage|	String| 	응답 메시지. 성공 시 "SUCCESS", 실패 시 오류 메시지 반환 |

### 1. User

#### 1.1. User 등록

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|users|	List|	Yes|	User 등록 정보|
|users[0].userId|	String|	Yes|	User ID <br/> 최대 48글자까지 등록 가능합니다. <br/> -\_@. 특수문자를 사용할 수 있으며, ID의 시작과 끝은 반드시 문자 및 숫자가 와야 합니다.|
|users[0].description|	String|	Yes|	User 설명 <br/> 최대 128글자까지 등록 가능합니다.|
|users[0].relations|	List|	No|	User - Role 관계 리스트|
|users[0].relations[0].roleId|	String|	Yes|	Role ID|
|users[0].relations[0].scopeId|	String|	Yes|	Scope ID|
|users[0].relations[0].validStartDate|	Date|	No| 	User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료) |
|users[0].relations[0].validEndDate|	Date|	No| 	User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료) |

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

|Key|	Type| 	Description                            |
|---|---|-----------------------------------------|
|errors|	List| 	오류 리스트, 오류가 발생하지 않았다면 빈 리스트를 반환합니다. |
|errors[0].code|	int| 	오류 코드                                  |
|errors[0].message|	String| 	오류 메시지                                 |

#### 1.2. User 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|user|	Object|	User 정보|
|user.appKey|	String|	앱키|
|user.userId|	String|	User ID|
|user.description|	String|	User 설명|
|user.regYmdt|	Timestamp|	등록일|

#### 1.3. User 리스트 조회

Scope ID와 Role ID를 넘겨주면, 해당 역할을 가진 User만 반환합니다.
includeRelation 을 true로 설정하면, Role ID와 연관 관계에 있는 Role 을 가진 User도 포함하여 반환합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|users|	List|	User 정보 리스트|
|users[0].appKey|	String|	앱키|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User 설명|
|users[0].regYmdt|	Timestamp|	등록일|
|users[0].relations | List | User에 할당된 관계 리스트 |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료)|
|users[0].relations[0].validEndDate | Date | User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료)|

#### 1.4. 벌크 User 리스트 조회

User 정보를 한번에 조회하는 API

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|users|	List|	User 정보 리스트|
|users[0].appKey|	String|	앱키|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User 설명|
|users[0].regYmdt|	Timestamp|	등록일|
|users[0].relations | List | User에 할당된 관계 리스트 |
|users[0].relations[0].userId | String | User ID |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료) |
|users[0].relations[0].validEndDate | Date | User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료) |


#### 1.5. User 설명 수정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|userId|	User ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	User 설명|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 1.6. User 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|userId|	User ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 1.7. 권한 체크

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
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|resources|	List|	Yes|	권한 체크 할 Resource 리스트|
|resources[0].operationId|	String|	Yes|	Operation ID|
|resources[0].resourceId|	String|	No|	Resource ID, Resource ID와 Path 중 하나의 값은 반드시 넣어야 합니다.|
|resources[0].resourcePath|	String|	No|	Resource Path, Resource ID와 Path 중 하나의 값은 반드시 넣어야 합니다.|
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
|authorizations|	List|	권한 체크 결과 리스트|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].permission|	boolean|	권한 체크 결과|
|authorizations[0].resourceId|	String|	Resource ID|
|authorizations[0].resourcePath|	String|	Resource Path|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.8. Role 권한 체크

User에 Role이 부여됬는지 여부를 반환합니다. 연관 관계에 따른 Role도 포함합니다.

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
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|roles|	List|	Yes|	권한 체크 할 Role 리스트|
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
|authorizations|	List|	권한 체크 결과 리스트|
|authorizations[0].permission|	boolean|	권한 체크 결과|
|authorizations[0].roleId|	String|	Role ID|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.9. User에 부여된 Role 조회

직접적으로 부여한 Role만 반환합니다. Role의 연관 관계에 따른 Role은 반환하지 않습니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|relations|	List|	User - Role 관계 리스트|
|relations[0].appKey|	String|	앱키|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|
|relations[0].validStartDate|	Date|User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료)|
|relations[0].validEndDate|	Date|User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료)|

#### 1.10. User에 Role 부여

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|createUserIfNotExist| Boolean| No| User가 없을때 User를 생성할 지 여부|
|validStartDate|	Date|	No|	User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료)|
|validEndDate|	Date|	No|	User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료) |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 1.11. User에 부여된 Role 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|userId|	User ID|

**[Query Parameter]**

|Key|	Value|	Required|
|---|---|---|
|roleId|	Role ID|	Yes|
|scopeId|	Scope ID|	Yes|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 1.12. User 의 기존 Role 삭제 후, 신규 Role 부여

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|relations|	List|	No|	User - Role 관계 리스트|
|relations[0].roleId|	String|	Yes|	Role ID|
|relations[0].scopeId|	String|	Yes|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```
#### 1.13. User에게 부여된 Role에 유효 기간 설정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles/valid-period|


**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|validStartDate|	Date|	No|	User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료)|
|validEndDate|	Date|	No|	User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료)|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

### 2. Scope

#### 2.1. Scope 등록

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

**[Request Body]**

```json
{
	"description": "",
	"scopeId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|scopeId|	String|	Yes|	Scope ID <br/> 최대 32글자까지 등록 가능합니다. </br> -\_ 특수문자를 사용할 수 있으며, ID의 시작과 끝은 반드시 문자 및 숫자가 와야 합니다. |
|description|	String|	Yes|	Scope 설명 <br/> 최대 128글자까지 등록 가능합니다.|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 2.2. Scope 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|scope|	Object|	Scope 정보|
|scope.appKey|	String|	앱키|
|scope.scopeId|	String|	Scope ID|
|scope.description|	String|	Scope 설명|

#### 2.3. Scope 설명 수정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|scopeId|	Scope ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Scope 설명|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 2.4. Scope 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|scopeId|	Scope ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 2.5. Scope과 연관된 연관 관계 조회

Scope ID와 관련된 연관 관계를 조회합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scope/{scopeId}/relations

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|relations|	List|	User - Role 관계 리스트|
|relations[0].appKey|	String|	앱키|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|


#### 2.6. Scope 리스트 조회

페이지 형태로 리스트를 조회할 수 있습니다.
page에 1, itemsPerPage에 10을 입력하면 처음 10개의 리스트를 조회합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

**[Query Parameter]**

|Key|	Value|	Required|	Description|
|---|---|---|---|
|scopeId|	Scope ID|	No|	|
|description|	|	No|	설명|
|page|  |	No|	검색을 원하는 페이지 번호로 1부터 시작|
|itemsPerPage|  |	No|	결과를 원하는 scopes 의 레코드 수|

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
|scopes|	List|	Scope 정보|
|scopes[0].description|	String|	Scope 설명|
|scopes[0].scopeId|	String|	Scope ID|
|totalItems|	int|	총 scope 수|

### 3. Role

#### 3.1. Role 등록

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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

|Key|	Type|	Required| 	Description                                                                                                  |
|---|---|---|---------------------------------------------------------------------------------------------------------------|
|roleId|	String|	Yes| 	Role ID <br/> 최대 128글자까지 등록 가능합니다. <br/> `-`, `_`, `.`, `:` 특수문자를 사용할 수 있으며, ID의 시작과 끝은 반드시 문자 및 숫자가 와야 합니다. |
|description|	String|	Yes| 	Role 설명 <br/> 최대 128글자까지 등록 가능합니다.                                                                           |
|roleName|	String|	No| 	Role 이름 <br/> 의미 있는 이름을 부여할 수 있습니다. 최대 128글자까지 등록 가능합니다.                                                     |
|roleGroup|	String|	No| 	Role Group <br/> Role들을 그룹핑하여 관리 목적으로 사용할 수 있습니다. 최대 128글자까지 등록 가능합니다.                                       |
|exposureOrder|	int|	No| 	노출 순서 <br/> 숫자만 가능합니다. 기본값은 0입니다.                                                                            |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.2. Role 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|
|roleName| Role 이름|
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
|role|	Object|	Role 정보|
|role.appKey|	String|	앱키|
|role.roleId|	String|	Role ID|
|role.description|	String|	Role 설명|
|role.roleName|	String|	Role 이름|
|role.roleGroup|	String|	Role 그룹 이름|
|role.exposureOrder|	int|	노출 순서|
|role.regDateTime|	String|	등록일시|
|role.roleTags|	Object|	Tag 정보 |
|role.roleTags.roleTagId|	String|	Tag ID|

#### 3.3. Role 정보 수정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|description|	String|	Yes|	User 설명|
|roleName|	String|	No|	Role 이름 <br/> 의미 있는 이름을 부여할 수 있습니다. 최대 128글자까지 등록 가능합니다.|
|roleGroup|	String|	No|	Role Group <br/> Role들을 그룹핑하여 관리 목적으로 사용할 수 있습니다. 최대 128글자까지 등록 가능합니다.|
|exposureOrder|	int | No | 노출 순서|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.4. Role 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|Delete|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|


**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.5. Role 연관 관계 설정

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"relatedRoleId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|relatedRoleId|	String|	Yes|	연관 관계를 설정 할 Role ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.6. Role 연관 관계 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations/{relatedRoleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|
|relatedRoleId|	연관 Role ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.7. Role에 User 할당


**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|createUserIfNotExist| Boolean| No| User 가 없을때 User를 생성할 지 여부|
|users|	List|	Yes|	User 리스트|
|users[0].scopeId|	String|	No|	Scope ID, 없을 시 기본값 ALL|
|users[0].userId|	String|	Yes|	User ID|
|users[0].validStartDate|	Date|	No|	User에게 부여된 Role의 유효 기간 시작 날짜(2024-02-27 이후 지원 종료)|
|users[0].validEndDate|	Date|	No|	User에게 부여된 Role의 유효 기간 종료 날짜(2024-02-27 이후 지원 종료)|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.8. Role 리스트 조회

페이지 형태로 리스트를 조회할 수 있습니다.
page에 1, itemsPerPage에 10을 입력하면 처음 10개의 리스트를 조회합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

**[Query Parameter]**

|Key|	Value| Required |
|---|---|---|
|roleId|	Role ID| No |
|description|	설명|	No|
|roleName|	Role 이름|	No|
|roleGroup|	Role Group 이름|	No|
|roleTagIds| Tag Id 조건(;는 OR, ,는 AND)|	No|
|page|  검색을 원하는 페이지 번호로 1부터 시작|	No|
|itemsPerPage|  결과를 원하는 scopes의 레코드 수|	No|

roleTagIds를 통해서 검색 시 Role에 설정 한 Tag를 AND 또는 OR 조건으로 조회할 수 있습니다.
예를 들어 Role에 A와 B Tag를 가지고 있는 Role을 검색 시에는 A;B로 조건을 만들 수 있고,
A 나 B Tag 중 하나만 있어도 검색을 하고 싶다면 A,B로 조건을 만들 수 있습니다.
(A;B),C와 같은 조건 생성도 가능합니다.

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
|roles|	List|	Role 정보|
|roles[0].description|	String|	Role 설명|
|roles[0].relatedRoleIds|	List|	연관 Role ID 목록|
|roles[0].roleId|	String|	Role ID|
|roles[0].roleName|	String|	Role 이름|
|roles[0].roleGroup|	String|	Role 그룹 이름|
|roles[0].exposureOrder|	int|	노출 순서|
|roles[0].regDateTime|	String|	등록일시|
|roles[0].roleTags|	Object|	Tag 정보 |
|roles[0].roleTags.roleTagId|	String|	Tag ID|
|totalItems|	int|	총 Role 수|


#### 3.9. Role Tag 생성

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"roleTagId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleTagId|	String|	Yes|	부여할 Tag ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```


#### 3.10. Role Tag 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags/{roleTagId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|roleId|	Role ID|
|roleTagId|	Tag ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 3.11. Role Tag 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|roleTags|	List|	Tag 정보|
|roleTags[0].roleTagId|	String|	Tag ID|

### 4. Resource

#### 4.1. Resource 생성

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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

|Key|	Type|	Required| 	Description                                                                                                                       |
|---|---|---|------------------------------------------------------------------------------------------------------------------------------------|
|resourceId|	String|	Yes| 	Resource ID <br/> 최대 32글자까지 등록 가능합니다. <br/> -\_ 특수문자를 사용할 수 있으며, ID의 시작과 끝은 반드시 문자 및 숫자가 와야 합니다.                                  |
|name|	String|	No| 	필요 없음.                                                                                                                            |야
|path|	String|	Yes| 	Resource 경로 <br/> 최대 1024글자까지 등록 가능합니다. <br/> Resource 경로는 '/'의 조합으로 이루어져야 합니다. <br/> 예외적으로 Path Variable을 표현할 수 있는 {}가 올 수 있습니다. |
|description|	String|	Yes| 	Resource 설명 <br/> 최대 128글자까지 등록 가능합니다.                                                                                            |
|priority|	smallint|	Yes| 	같은 경로에서 보여지는 우선순위 <br/> -32768~32767 값이 올 수 있으며, 낮을수록 앞에 보이게 됩니다.                                                              |
|metadata|	String|	Yes| 	사용자 정의 데이터 <br/> 최대 65536글자까지 등록 가능합니다.                                                                                           |
|uiPath|	String|	Yes| 	UI Path 경로 <br/> 최대 1024글자까지 등록 가능합니다. <br/> UI Path 경로는 Resource 이름과 '/'의 조합으로 이루어져야 합니다.                                        |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 4.2. Resource Hierarchy 조회

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
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|resources|	List|	Resource 리스트|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].description|	String|	Resource 설명|
|resources[0].name|	String|	Resource 이름|
|resources[0].path|	String|	Resource 경로|
|resources[0].priority|	smallint|	우선순위|
|resources[0].metadata|	String|	사용자 정의 데이터|
|resources[0].resources|	List|	Resource 리스트|

#### 4.3. Resource 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|resource|	Object|	Resource 정보|
|resource.appKey|	String|	앱키|
|resource.resourceId|	String|	Resource ID|
|resource.description|	String|	Resource 설명|
|resource.name|	String|	Resource 이름|
|resource.path|	String|	Resource 경로|
|resource.priority|	smallint|	우선순위|
|resource.metadata|	String|	사용자 정의 데이터|

#### 4.4. Resource 수정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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

|Key|	Type|	Required| 	Description                                                                                                                       |
|---|---|---|------------------------------------------------------------------------------------------------------------------------------------|
|name|	String|	No| 필요 없음.                                                                                                                             |
|path|	String|	Yes| 	Resource 경로 <br/> 최대 1024글자까지 등록 가능합니다. <br/> Resource 경로는 '/'의 조합으로 이루어져야 합니다. <br/> 예외적으로 Path Variable을 표현할 수 있는 {}가 올 수 있습니다. |
|description|	String|	Yes| 	Resource 설명 <br/> 최대 128글자까지 등록 가능합니다.                                                                                            |
|priority|	smallint|	Yes| 	같은 경로에서 보여지는 우선순위 <br/> -32768~32767 값이 올 수 있으며, 낮을수록 앞에 보이게 됩니다.                                                               |
|metadata|	String|	Yes| 	사용자 정의 데이터 <br/> 최대 65536글자까지 등록 가능합니다.                                                                                           |
|uiPath|	String|	Yes| 	UI Path 경로 <br/> 최대 1024글자까지 등록 가능합니다. <br/> UI Path 경로는 Resource 이름과 '/'의 조합으로 이루어져야 합니다.                                        |

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 4.5. Resource 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|resourceId|	Resource ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 4.6. Resource와 관련된 권한 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|authorizations|	List|	권한 정보 리스트|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].roleId|	String|	Role ID|

#### 4.7. Resource에 권한을 추가합니다.

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|scopeId|	String|	No|	Scope ID, 없을 시 기본값 ALL|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 4.8. Resource 리스트 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|resources|	List|	Resource 정보|
|resources[0].description|	String|	Resource 설명|
|resources[0].metadata|	String|	사용자 정의 데이터|
|resources[0].name|	String|	Resource 이름|
|resources[0].path|	String|	Resource 경로|
|resources[0].priority|	smallint|	우선순위|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].uiPath|	String|	uiPath|

### 5. Operation

#### 5.1. Operation 등록

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

**[Request Body]**

```json
{
	"description": "",
	"operationId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|operationId|	String|	Yes|	Operation ID <br/> 최대 32글자까지 등록 가능합니다. <br/> -\_ 특수문자를 사용할 수 있으며, ID의 시작과 끝은 반드시 문자 및 숫자가 와야 합니다.|
|description|	String|	Yes|	Operation 설명 <br/> 최대 128글자까지 등록 가능합니다.|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 5.2. Operation 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
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
|operation|	Object|	Operation 정보|
|operation.appKey|	String|	앱키|
|operation.operationId|	String|	Operation ID|
|operation.description|	String|	Operation 설명|

#### 5.3. Operation 설명 수정

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|operationId|	Operation ID|

**[Request Body]**


```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Operation 설명|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```

#### 5.4. Operation 삭제

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|
|operationId|	Operation ID|

**[Response Body]**

```json
{
	"header" : {
		"isSuccessful" : true,
		"resultCode": 0,
		"resultMessage" : "Success."
	}
}
```


#### 5.5. Operation 리스트 조회

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE] 에서 발급받은 SecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE] 에서 발급받은 앱키|

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
|operations|	List|	Operation 정보|
|operations[0].appKey|	String|	앱키|
|operations[0].description|	String|	Operation 설명|
|operations[0].operationId|	String|	Operation ID|
