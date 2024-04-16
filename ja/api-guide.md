## Application Service > ROLE > APIガイド


> ROLEサービスを利用して権限をチェックするためには
> RESTful APIを呼び出すか、クライアントSDKを利用する必要があります。

## アプリケーションキー&秘密鍵

RESTful APIとクライアントSDKを使用するには、アプリケーションキーと秘密鍵が必要です。
[CONSOLE] 右上の**URL & Appkey**ボタンをクリックすると、発行キー情報を確認できます。

![[図1]アプリケーションキー&秘密鍵確認](http://static.toastoven.net/prod_role/role_60.png)
<center>[図1]アプリケーションキー&秘密鍵の確認</center>

## RESTful APIガイド

### Common Response Body

すべてのAPIリクエストに対してHTTPレスポンスコードは200でレスポンスします。
詳細なレスポンス結果はResponse Bodyのheader項目を参照してください。

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
|header|	Object| 	レスポンスヘッダ                                 |
|header.isSuccessful|	boolean| 	成否                                 |
|header.resultCode|	int| 	レスポンスコード。成功時は0、失敗時はエラーコードを返す           |
|header.resultMessage|	String| 	レスポンスメッセージ。成功時は"SUCCESS"、失敗時はエラーメッセージを返す |

### 1. User

#### 1.1. User登録

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|users|	List|	Yes|	User登録情報|
|users[0].userId|	String|	Yes|	User ID <br/> 最大48文字まで登録できます。 <br/> -\_@. 特殊文字を使用することができ、IDの先頭と末尾は必ず文字及び数字で構成されている必要があります。|
|users[0].description|	String|	Yes|	User説明 <br/> 最大128文字まで登録できます。|
|users[0].relations|	List|	No|	User - Role関係リスト|
|users[0].relations[0].roleId|	String|	Yes|	Role ID|
|users[0].relations[0].scopeId|	String|	Yes|	Scope ID|
|users[0].relations[0].validStartDate|	Date|	No| 	Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了) |
|users[0].relations[0].validEndDate|	Date|	No| 	Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了) |

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
|errors|	List| 	エラーリスト。エラーが発生しなかった場合は空のリストを返します。 |
|errors[0].code|	int| 	エラーコード                                 |
|errors[0].message|	String| 	エラーメッセージ                                |

#### 1.2. User照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|user|	Object|	User情報|
|user.appKey|	String|	アプリケーションキー|
|user.userId|	String|	User ID|
|user.description|	String|	User説明|
|user.regYmdt|	Timestamp|	登録日|

#### 1.3. Userリストの照会

Scope IDとRole IDを渡したら、そのロールを持っているUserだけ返します。
includeRelationをtrueに設定すると、Role IDと関連関係にあるRoleを持つUser も含めて返します。

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|users|	List|	User情報リスト|
|users[0].appKey|	String|	アプリケーションキー|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User説明|
|users[0].regYmdt|	Timestamp|	登録日|
|users[0].relations | List | Userに割り当てられた関係リスト |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了)|
|users[0].relations[0].validEndDate | Date | Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了)|

#### 1.4. バルクUserリストの照会

User情報を一度に照会するAPI

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|users|	List|	User情報リスト|
|users[0].appKey|	String|	アプリケーションキー|
|users[0].userId|	String|	User ID|
|users[0].description|	String|	User説明|
|users[0].regYmdt|	Timestamp|	登録日|
|users[0].relations | List | Userに割り当てられた関係リスト |
|users[0].relations[0].userId | String | User ID |
|users[0].relations[0].roleId | String | Role ID |
|users[0].relations[0].scopeId | String | Scope ID |
|users[0].relations[0].validStartDate | Date | Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了) |
|users[0].relations[0].validEndDate | Date | Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了) |


#### 1.5. User説明の修正

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|userId|	User ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	User説明|

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

#### 1.6. User削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 1.7. 権限チェック

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
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|resources|	List|	Yes|	権限チェックするResourceリスト|
|resources[0].operationId|	String|	Yes|	Operation ID|
|resources[0].resourceId|	String|	No|	Resource ID, Resource IDとPathのいずれかの値は必ず入れてください。|
|resources[0].resourcePath|	String|	No|	Resource Path, Resource IDとPathのいずれかの値は必ず入れてください。|
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
|authorizations|	List|	権限チェック結果リスト|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].permission|	boolean|	権限チェック結果|
|authorizations[0].resourceId|	String|	Resource ID|
|authorizations[0].resourcePath|	String|	Resource Path|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.8. Role権限チェック

UserにRoleが付与されているかどうかを返します。関連関係に応じたRoleも含みます。

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
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|roles|	List|	Yes|	権限チェックするRoleリスト|
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
|authorizations|	List|	権限チェック結果リスト|
|authorizations[0].permission|	boolean|	権限チェック結果|
|authorizations[0].roleId|	String|	Role ID|
|authorizations[0].scopeId|	String|	Scope ID|

#### 1.9. Userに付与されたRole照会

直接付与したRoleだけを返します。Roleの関連関係に基づくRoleは返しません。

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|relations|	List|	User - Role関係リスト|
|relations[0].appKey|	String|	アプリケーションキー|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|
|relations[0].validStartDate|	Date|Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了)|
|relations[0].validEndDate|	Date|Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了)|

#### 1.10. UserにRole付与

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|createUserIfNotExist| Boolean| No| Userが存在しない場合、Userを作成するかどうか|
|validStartDate|	Date|	No|	Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了)|
|validEndDate|	Date|	No|	Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了) |

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

#### 1.11. Userに付与されたRole削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 1.12. Userの既存Role削除後、新規Role付与

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|relations|	List|	No|	User - Role関係リスト|
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
#### 1.13. Userに付与されたRoleの有効期間設定

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/users/{userId}/roles/valid-period|


**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|validStartDate|	Date|	No|	Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了)|
|validEndDate|	Date|	No|	Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了)|

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

#### 2.1. Scope登録

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

**[Request Body]**

```json
{
	"description": "",
	"scopeId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|scopeId|	String|	Yes|	Scope ID <br/> 最大32文字まで登録できます。 </br> -\_特殊文字を使用することができ、IDの先頭と末尾は必ず文字及び数字で構成されている必要があります。|
|description|	String|	Yes|	Scope説明 <br/> 最大128文字まで登録できます。|

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

#### 2.2. Scope照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|scope|	Object|	Scope情報|
|scope.appKey|	String|	アプリケーションキー|
|scope.scopeId|	String|	Scope ID|
|scope.description|	String|	Scope説明|

#### 2.3. Scope説明の修正

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|scopeId|	Scope ID|

**[Request Body]**

```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Scope説明|

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

#### 2.4. Scopeの削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/scopes/{scopeId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 2.5. Scopeと関連する関連関係の照会

Scope IDと関連する関連関係を照会します。

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scope/{scopeId}/relations

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|relations|	List|	User - Role関係リスト|
|relations[0].appKey|	String|	アプリケーションキー|
|relations[0].roleId|	String|	Role ID|
|relations[0].scopeId|	String|	Scope ID|
|relations[0].userId|	String|	User ID|


#### 2.6. Scopeリストの照会

ページ形式でリストを照会できます。
pageに1、itemsPerPageに10を入力すると、最初の10個のリストを照会します。

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/scopes|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

**[Query Parameter]**

|Key|	Value|	Required|	Description|
|---|---|---|---|
|scopeId|	Scope ID|	No|	|
|description|	|	No|	説明|
|page|  |	No|	検索したいページ番号。1から始まる|
|itemsPerPage|  |	No|	結果を求める scopesのレコード数|

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
|scopes|	List|	Scope情報|
|scopes[0].description|	String|	Scopeの説明|
|scopes[0].scopeId|	String|	Scope ID|
|totalItems|	int|	scopeの総数|

### 3. Role

#### 3.1. Roleの登録

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|roleId|	String|	Yes| 	Role ID <br/> 最大128文字まで登録できます。 <br/> `-`, `_`, `.`, `:`特殊文字を使用することができ、IDの先頭と末尾は必ず文字及び数字で構成されている必要があります。|
|description|	String|	Yes| 	Roleの説明 <br/> 最大128文字まで登録できます。                                                                           |
|roleName|	String|	No| 	Roleの名前 <br/> 意味のある名前を付けることができます。最大128文字まで登録できます。                                                     |
|roleGroup|	String|	No| 	Role Group <br/> Roleをグルーピングして管理目的で使用できます。最大128文字まで登録できます。                                       |
|exposureOrder|	int|	No| 	表示順序 <br/> 数字のみ可能です。デフォルト値は0です。                                                                            |

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

#### 3.2. Role照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|roleId|	Role ID|
|roleName| Roleの名前|
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
|role|	Object|	Role情報|
|role.appKey|	String|	アプリケーションキー|
|role.roleId|	String|	Role ID|
|role.description|	String|	Roleの説明|
|role.roleName|	String|	Roleの名前|
|role.roleGroup|	String|	Roleグループ名|
|role.exposureOrder|	int|	表示順序|
|role.regDateTime|	String|	登録日|
|role.roleTags|	Object|	Tag情報 |
|role.roleTags.roleTagId|	String|	Tag ID|

#### 3.3. Role情報の修正

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|description|	String|	Yes|	User説明|
|roleName|	String|	No|	Roleの名前 <br/> 意味のある名前を付けることができます。最大128文字まで登録できます。|
|roleGroup|	String|	No|	Role Group <br/> Roleをグルーピングして管理目的で使用できます。最大128文字まで登録できます。|
|exposureOrder|	int | No | 表示順序|

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

#### 3.4. Roleの削除

**[Method, URL]**

|Method|	URI|
|---|---|
|Delete|	/role/v1.0/appkeys/{appKey}/roles/{roleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 3.5. Role 関連関係の設定

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"relatedRoleId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|relatedRoleId|	String|	Yes|	関連関係を設定するRole ID|

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

#### 3.6. Role 関連関係の削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/relations/{relatedRoleId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|roleId|	Role ID|
|relatedRoleId|	関連Role ID|

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

#### 3.7. RoleにUserを割り当てる


**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/users|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|createUserIfNotExist| Boolean| No| User が存在しない場合、User作成するかどうか|
|users|	List|	Yes|	Userリスト|
|users[0].scopeId|	String|	No|	Scope ID。ない場合はデフォルト値のALL|
|users[0].userId|	String|	Yes|	User ID|
|users[0].validStartDate|	Date|	No|	Userに付与されたRoleの有効期間開始日(2024-02-27以降サポート終了)|
|users[0].validEndDate|	Date|	No|	Userに付与されたRoleの有効期間終了日(2024-02-27以降サポート終了)|

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

#### 3.8. Roleリストの照会

ページ形式でリストを照会できます。
pageに1、itemsPerPageに10を入力すると、最初の10個のリストを照会します。

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

**[Query Parameter]**

|Key|	Value| Required |
|---|---|---|
|roleId|	Role ID| No |
|description|	説明|	No|
|roleName|	Roleの名前|	No|
|roleGroup|	Role Groupの名前|	No|
|roleTagIds| Tag Id条件(;はOR。,はAND)|	No|
|page| 検索したいページ番号。1から始まる|	No|
|itemsPerPage| 結果したいscopesのレコード数|	No|

roleTagIdsを使って検索する時、Roleに設定したTagをANDまたはOR条件で照会できます。
例えば、RoleにAとB Tagを持っているRoleを検索する時はA;Bで条件を作ることができます、
AかB Tagのどちらか一つだけあっても検索をしたい場合は、A,Bで条件を作ることができます。
(A;B),Cなどの条件も作成できます。

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
|roles|	List|	Role情報|
|roles[0].description|	String|	Roleの説明|
|roles[0].relatedRoleIds|	List|	関連Role IDリスト|
|roles[0].roleId|	String|	Role ID|
|roles[0].roleName|	String|	Roleの名前|
|roles[0].roleGroup|	String|	Roleグループ名|
|roles[0].exposureOrder|	int|	表示順序|
|roles[0].regDateTime|	String|	登録日|
|roles[0].roleTags|	Object|	Tag情報 |
|roles[0].roleTags.roleTagId|	String|	Tag ID|
|totalItems|	int|	総Role数|


#### 3.9. Role Tagの作成

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|roleId|	Role ID|

**[Request Body]**

```json
{
	"roleTagId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|roleTagId|	String|	Yes|	付与するTag ID|

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


#### 3.10. Role Tagの削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags/{roleTagId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 3.11. Role Tagの照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/roles/{roleId}/tags|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|roleTags|	List|	Tag情報|
|roleTags[0].roleTagId|	String|	Tag ID|

### 4. Resource

#### 4.1. Resourceの作成

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|resourceId|	String|	Yes| 	Resource ID <br/> 最大32文字まで登録できます。 <br/> -\_特殊文字を使用することができ、IDの先頭と末尾は必ず文字及び数字で構成されている必要があります。                                  |
|name|	String|	No| 	必要なし                                                                                                                            |
|path|	String|	Yes| 	Resourceパス <br/> 最大1024文字まで登録できます。 <br/> Resourceパスは'/'の組み合わせで構成される必要があります。<br/>例外的にPath Variableを表現できる{}を使うことができます。 |
|description|	String|	Yes| 	Resourceの説明 <br/> 最大128文字まで登録できます。                                                                                            |
|priority|	smallint|	Yes| 	同じパスで表示される優先順位<br/> -32768～32767の値を指定することができ、小さいほど前に表示されます。                                                              |
|metadata|	String|	Yes| 	ユーザー定義データ <br/> 最大65536文字まで登録できます。                                                                                           |
|uiPath|	String|	Yes| 	UI Pathパス <br/> 最大1024文字まで登録できます。 <br/> UI PathパスはResource名と'/'の組み合わせで構成されている必要があります。                                        |

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

#### 4.2. Resource Hierarchy照会

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
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|resources|	List|	Resourceリスト|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].description|	String|	Resourceの説明|
|resources[0].name|	String|	Resourceの名前|
|resources[0].path|	String|	Resourceパス|
|resources[0].priority|	smallint|	優先順位|
|resources[0].metadata|	String|	ユーザー定義データ|
|resources[0].resources|	List|	Resourceリスト|

#### 4.3. Resource照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|resource|	Object|	Resource情報|
|resource.appKey|	String|	アプリケーションキー|
|resource.resourceId|	String|	Resource ID|
|resource.description|	String|	Resourceの説明|
|resource.name|	String|	Resourceの名前|
|resource.path|	String|	Resourceパス|
|resource.priority|	smallint|	優先順位|
|resource.metadata|	String|	ユーザー定義データ|

#### 4.4. Resource修正

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|name|	String|	No| 必要なし                                                                                                                             |
|path|	String|	Yes| 	Resourceパス <br/> 最大1024文字まで登録できます。 <br/> Resourceパスは'/'の組み合わせで構成される必要があります。<br/>例外的にPath Variableを表現できる{}を使うことができます。 |
|description|	String|	Yes| 	Resourceの説明 <br/> 最大128文字まで登録できます。                                                                                            |
|priority|	smallint|	Yes| 	同じパスで表示される優先順位 <br/> -32768～32767の値を指定することができ、小さいほど前に表示されます。                                                               |
|metadata|	String|	Yes| 	ユーザー定義データ <br/> 最大65536文字まで登録できます。                                                                                           |
|uiPath|	String|	Yes| 	UI Pathパス <br/> 最大1024文字まで登録できます。 <br/> UI PathパスはResource名と'/'の組み合わせで構成されている必要があります。                                        |

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

#### 4.5. Resource削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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

#### 4.6. Resourceと関連する権限の照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|authorizations|	List|	権限情報リスト|
|authorizations[0].operationId|	String|	Operation ID|
|authorizations[0].roleId|	String|	Role ID|

#### 4.7. Resourceに権限を追加します。

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/resources/{resourceId}/authorizations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|scopeId|	String|	No|	Scope ID。ない場合はデフォルト値ALL|

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

#### 4.8. Resourceリストの照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/resources|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|resources|	List|	Resource情報|
|resources[0].description|	String|	Resourceの説明|
|resources[0].metadata|	String|	ユーザー定義データ|
|resources[0].name|	String|	Resourceの名前|
|resources[0].path|	String|	Resourceパス|
|resources[0].priority|	smallint|	優先順位|
|resources[0].resourceId|	String|	Resource ID|
|resources[0].uiPath|	String|	uiPath|

### 5. Operation

#### 5.1. Operationの登録

**[Method, URL]**

|Method|	URI|
|---|---|
|POST|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

**[Request Body]**

```json
{
	"description": "",
	"operationId": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|operationId|	String|	Yes|	Operation ID <br/> 最大32文字まで登録できます。 <br/> -\_特殊文字を使用することができ、IDの先頭と末尾は必ず文字及び数字で構成されている必要があります。|
|description|	String|	Yes|	Operation説明 <br/> 最大128文字まで登録できます。|

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

#### 5.2. Operation照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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
|operation|	Object|	Operation情報|
|operation.appKey|	String|	アプリケーションキー|
|operation.operationId|	String|	Operation ID|
|operation.description|	String|	Operation説明|

#### 5.3. Operation説明の修正

**[Method, URL]**

|Method|	URI|
|---|---|
|PUT|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
|operationId|	Operation ID|

**[Request Body]**


```json
{
	"description": ""
}
```

|Key|	Type|	Required|	Description|
|---|---|---|---|
|description|	String|	Yes|	Operationの説明|

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

#### 5.4. Operationの削除

**[Method, URL]**

|Method|	URI|
|---|---|
|DELETE|	/role/v1.0/appkeys/{appKey}/operations/{operationId}|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|
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


#### 5.5. Operationリストの照会

**[Method, URL]**

|Method|	URI|
|---|---|
|GET|	/role/v1.0/appkeys/{appKey}/operations|

**[Request Header]**

|Key|	Value|
|---|---|
|X-Secret-Key|	[CONSOLE]で発行されたSecretKey|
|Content-Type|	application/json|

**[Path Variable]**

|Key|	Value|
|---|---|
|appKey|	[CONSOLE]で発行されたアプリケーションキー|

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
|operations|	List|	Operation情報|
|operations[0].appKey|	String|	アプリケーションキー|
|operations[0].description|	String|	Operation説明|
|operations[0].operationId|	String|	Operation ID|
