## Application Service > ROLE > SDK使用ガイド

> ROLEサービスを利用して権限をチェックするためには
> RESTful APIを呼び出すか、クライアントSDKを利用する必要があります。

## アプリケーションキー&秘密鍵

RESTful APIとクライアントSDKを使用するには、アプリケーションキーと秘密鍵が必要です。 
[CONSOLE]の右上で発行されたキー情報を確認できます。

![[図1]アプリケーションキー&秘密鍵の確認](http://static.toastoven.net/prod_role/role_60.png)
<center>[図1]アプリケーションキー&秘密鍵の確認</center>

## クライアントSDK

### クライアントSDKとは？

RESTful APIを簡単に呼び出すためのROLE専用クライアントSDKです。
独自のキャッシュ機能を持っているため、より効率的にROLEサービスを利用できます。
現在はJAVA言語のみサポートしています。

### 使用環境
`JDK 11`バージョン以上の環境

### Mavenを利用したJAVAクライアントSDKの使用方法

JAVAクライアントSDKを使用するにはpom.xmlにmaven repository及びdepencencyを設定する必要があります。

**[Maven Repository]**
Maven Central Repositoryに保存されているので別途設定は必要ありません。
もし、他のリポジトリを使う場合やMaven Centralが参照できない環境では次のように設定。

```xml
<repositories>
   <repository>
      <id>central</id>
      <url>https://repo.maven.apache.org/maven2</url>
   </repository>
</repositories>
```
**[Maven Dependency]**

```xml
<dependencies>
   <dependency>
      <groupId>com.nhncloud.role</groupId>
      <artifactId>nhncloud-role-sdk</artifactId>
      <version>2.0.0</version>
   </dependency>
</dependencies>
```

### JAVAクライアントSDK使用方法

JAVAクライアントSDKを使用するには、まずRoleClientFactoryオブジェクトを利用してRoleClientオブジェクトのインスタンスを作成する必要があります。
RoleClientオブジェクトを作成したら、そのオブジェクトで提供するメソッドを呼び出して様々な作業を処理します。

**[RoleConfig]**

| Key            | Type | Required | Description                                                         |
|--------------|----------------|----|---------------------------------------------------------------------|
| appKey         | String  |**Yes**| サーバーから発行されたアプリケーションキー                                                       |
| secretKey      | String  |**Yes**| サーバーから発行された秘密鍵                                                     |
| domain         | String  |**No**| ドメインアドレス<br/>デフォルトで設定された値を使用し、別途設定する必要はありません。                         |
| connectTimeout | Integer |**No**| 接続のタイムアウトを設定することができ、時間単位はミリ秒。<br/>デフォルト値はokHttpのデフォルト値である10秒。   |
| readTimeout    | Integer |**No**| Readタイムアウトを設定できます。単位はミリ秒で、デフォルトはokHttpのデフォルトである10秒です。 |

```java
String appKey = "appKey";
String secretKey = "secretKey";

// RoleClientオブジェクトを作成する正しい方法
// ドメインは別途設定する必要はありません。
RoleClient client = RoleClientFactory.getClient(RoleConfig.builder()
                                                            .appKey(appKey)
                                                            .secretKey(secretKey)
                                                            .connectTimeout(30_000)
                                                            .readTimeout(60_000)
                                                            .build());

// 以下のように直接コンストラクタを呼び出してはいけません。
RoleClient client = new RoleClient(RoleConfig.builder()
                                                .appKey(appKey)
                                                .secretKey(secretKey)
                                                .connectTimeout(30_000)
                                                .readTimeout(60_000)
                                                .build());
```

> RoleClientのコンストラクタを直接呼び出さないように注意してください。

### SDK使用ガイド
#### Common
> SDK共通機能で使う部分

1. ページングのためのリクエスト/レスポンスモデル

**[Pageable]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| page         | Integer| **No** | ページ番号        |
| itemsPerPage | Integer | **No** | リストごとに照会されるアイテムの数 |
| sort              | List&lt;String>      | **No** |   データソート基準    |

**[Page]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| totalItems         | Integer    | **Yes** | 全体数   |
| items | List&lt;T> | **Yes** | 照会されたリスト    |

#### 1. ユーザー
> ユーザー情報の登録、照会、修正、削除機能及びユーザーロールの変更履歴の照会

1. Model

**[User]**

|Key|    Type | Required | Description        |
|--------------|----------------|----|--------------------|
|userId|     String |**Yes**| ユーザーID             |
|description|    String  |**No**| 説明                |
|regYmdt|  OffsetDateTime |**No**| ユーザー作成日時。戻り時に設定される。 |
|roleCounts|  List&lt;UserRoleCount> |**No**| ユーザーに割り当てられたロール数    |
|roleRelations|  List&lt;UserRoleRelation> |**No**| 関連ロール             |

**[UserRoleCount]**

| Key       | Type     | Required | Description   |
|-----------|----------|----------|---------------|
| scopeId   | String   | **No**   | スコープID         |
| roleCount | Integer  | **No**   | スコープID別のロール数 |

**[UserRoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String            |**Yes**|    適用対象ID              |
|roleId|     String            |**Yes**|    ロールID                 |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|  ロール使用有無: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**| ロール条件属性             |
|regYmdt | Date| **Yes** | 作成日時  |

**[Condition]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId| String |**Yes**|  条件属性ID          |
|attributeOperatorType | Required |**Yes**|  条件属性演算子タイプ |
|attributeValues| List&lt;String> |**No**| 条件属性値          |

2. ユーザーの作成

```java
User user = User.builder()
                 .userId("")
                 .description("")
                 .roleRelations(List.of(UserRoleRelation.builder()
                                                        .roleId("")
                                                        .scopeId("")
                                                        .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                                        .conditions(List.of(Condition.builder()
                                                                                     .attributeId("")
                                                                                     .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                                     .attributeValues(List.of(""))))
                                                                                     .build()))
                                                        .build();

client.createUsers(List.of(user));
```

3. ユーザーの照会

**[GetUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**Yes**|   ユーザーID                                             |
| searchRoleOptionCode | SearchRoleOptionCode    |**No**|   関連関係権限を含めて検索するかどうか： DIRECT_ROLE, INDIRECT_ROLE |
| roleIds              | List&lt;String> |**No**|  ロールIDリスト     |
| scopeIds             | List&lt;String> |**No**|  適用対象IDリスト |

```java
GetUserRequest request = GetUserRequest.builder()
                                       .userId("")
                                       .build();

User user = client.getUser(request);
```

4. ユーザーリストの照会

⚠️レスポンス時に使用されるモデルは`Common`を参照

**[GetUserRequest]**

| Key                  |     Type | Required | Description                               |
|--------------|----------------|----|-------------------------------------------|
| userIds              | List&lt;String>      |**No**| ユーザーIDリスト(完全一致)                          |
| userIdPreLike        | String               |**No**| ユーザーID(前方一致)                             |
| scopeIds             | List&lt;String>      |**No**| スコープIDリスト(完全一致)                           |
| scopeIdPreLike       | String               |**No**| スコープID(前方一致)                              |
| roleIds              | List&lt;String>      |**No**| ロールIDリスト(完全一致)                           |
| roleIdPreLike        | String               |**No**| ロールID(前方一致)                              |
| descriptionLike      | String               |**No**| ユーザー説明(部分一致)                             |
| searchRoleOptionCode | SearchRoleOptionCode |**No**| アクセス可能なロールリストの検索方法                       |
| needRoleRelations    | Boolean              |**No**| レスポンス時にロール関連関係を含めるかどうか(デフォルト値: true)             |
| needRoleTags         | Boolean              |**No**| レスポンス時にロール関連関係を含める時、ロールタグを含めるかどうか(デフォルト値: false) |
| needRoleCount         | Boolean              |**No**| レスポンス時にユーザーが持っているロール数を含めるかどうか(デフォルト値: false)     |

```java
GetUserRequest request = GetUsersRequest.builder()
                                              .userId("")
                                              .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<User> user = client.getUsers(request, pageable);
```

5. ユーザーの修正

**[PutUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| user                 | User      |**Yes**|   ⚠️リクエスト時に使用されるモデルは`User`を参照 |
| createUserIfNotExist | Boolean    |**No**|  リクエスト時にユーザーが存在しない場合、作成するかどうか |


```java
User user = User.builder()
                 .userId("")
                 .description("")
                 .roleRelations(List.of(UserRoleRelation.builder()
                                                        .roleId("")
                                                        .scopeId("")
                                                        .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                                        .conditions(List.of(Condition.builder()
                                                                                     .attributeId("")
                                                                                     .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                                     .attributeValues(List.of(""))))
                                                                                     .build()))
                                                        .build();

PutUserRequest request = PutUserRequest.builder()
                                       .user(user)
                                       .createUserIfNotExist(true)
                                       .build();

client.updateUser(request);
```

6. ユーザーの削除

```java
String userId = "";

client.deleteUser(userId);
```

7. ユーザーの一括削除

**[DeleteUsersRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds             | Set&lt;String>      |**Yes**|  ユーザーIDリスト |

```java
DeleteUsersRequest request = DeleteUsersRequest.builder()
                                               .userIds(Set.of(""))
                                               .build();

client.deleteUsers(request);
```

8. ユーザーロール変更履歴リストの照会

**[GetUserRoleHistoriesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**No**|   ユーザーID      |
| roleId              | String |**No**|    ロールID       |
| scopeId             | String   |**No**| 適用対象ID
| fromDateTime | OffsetDateTime   |**No**| 変更開始日時    |
| toDateTime | OffsetDateTime   |**No**|   変更終了日時    |

```java
GetUserRoleHistoriesRequest request = GetUserRoleHistoriesRequest.builder()
                                                                .userId("")
                                                                .roleId("")
                                                                .build();

Page<UserRoleHistory> userRoleHistories = client.getUserRoleHistories(request, Pageable.builder()
                                                                                        .page(0)
                                                                                        .itemsPerPage(10)
                                                                                        .build());
```

**[UserRoleHistory]**

| Key                  |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| userHistorySeq               | long           |**Yes**|   順番           |
| userId               | String         |**Yes**|  ユーザーID      |
| roleId              | String         |**No**|   ロールID       |
| scopeId             | String          |**No**| 適用対象ID
| roleApplyPolicyCode | RoleApplyPolicyCode |**No**|  ロール使用有無: ALLOW, DENY     |
| conditions | List&lt;ConditionBundle> |**No**|  ロール条件属性    |
| command | UserRoleHistoryCommandCode |**Yes**|   コマンド    |
| executionTime | OffsetDateTime |**Yes**| 変更日時    |
| operatorUuid | String |**Yes**|  作業者UUID     |

9. スコープベースのユーザー修正

**[PutUserScopeRequest]**

| Key                  |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String         |**Yes**|  ユーザーID      |
| scopeId             | String          |**Yes**| 適用対象ID
| description|    String  |**No**| 説明|
| createUserIfNotExist | Boolean    |**No**|  リクエスト時にユーザーが存在しない場合、作成するかどうか |
| roleRelations|  List&lt;UserRoleRelation> |**No**| 関連ロール|

```java
PutUserRequest request = PutUserScopeRequest.builder()
                                            .userId("")
                                            .description("")
                                            .createUserIfNotExist(true)
                                            .roleRelations(List.of(UserRoleRelation.builder()
                                                                                   .roleId("")
                                                                                   .scopeId("")
                                                                                   .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                                                                   .conditions(List.of(Condition.builder()
                                                                                                                .attributeId("")
                                                                                                                .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                                                                .attributeValues(List.of(""))))
                                                                                                                .build()))
                                                                                   .build();

client.updateUserInScope(request);
```

#### 2. オペレーション
> Operation情報登録、照会、修正、削除

1. Model

**[Operation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationId|    String                    |**Yes**| オペレーションID|
|description|    String                    |**No**| 説明|

2. オペレーション作成

```java
Operation operation = Operation.builder()
                               .operationId("")
                               .description("")
                               .build();

client.createOperation(operation);
```

3. オペレーション照会

⚠️レスポンス時に使われるモデルは`Model`を参照

```java
String operationId = "";

Operation operation = client.getOperation(operationId);
```

4. オペレーションリストの照会

⚠️レスポンス時に使われるモデルは`Common`を参照

**[GetOperationsRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationIds|   List&lt;String> |**No**|  オペレーションIDリスト |
|operationIdPreLike|     String          |**No**|  オペレーションID(前方一致) |
|descriptionLike|    String          |**No**|  説明(部分一致)           |

```java
GetOperationsRequest request = GetOperationsRequest.builder()
                                                    .operationId("")
                                                    .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<Operation> operations = client.getOperations(request, pageable);
```

5. オペレーション修正

⚠️レスポンス時に使われるモデルは`Operation`を参照
```java
Operation operation = Operation.builder()
                .operationId("")
                .description("")
                .build();

client.updateOperation(operation);
```

6. オペレーションの削除

```java
String operationId = "";

client.deleteOperation(userId);
```

7. オペレーションの一括削除

**[DeleteOperationsRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| operationIds             | Set&lt;String>      |**Yes**|  オペレーションIDリスト |

```java
DeleteOperationsRequest request = DeleteOperationsRequest.builder()
                                                          .operationIds(Set.of(""))
                                                          .build();

client.deleteOperations(request);
```

#### 3. 属性
> 属性情報の登録、照会、修正、削除

1. Model

**[Attribute]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String          |**Yes**|  条件属性ID                                 |
|attributeName|  String          |**No**|  条件属性名                                |
|description|    String          |**No**|  説明                                          |
|attributeDataType | AttributeDataType |**Yes**|  条件属性データタイプ  |
|attributeCreationType | AttributeCreationType |**No**|  条件属性作成タイプ  |
|attributeTagIds|    List&lt;String>    |**No**|   条件属性タグリスト                             |
|attributeRoleRelationIds|   List&lt;String> |**No**|   関連ロールリスト                                    |

2. 属性の作成

```java
Attribute attribute = Attribute.builder()
                               .attributeId("")
                               .attributeName("")
                               .description("")
                               .attributeDataTypeCode(AttributeDataTypeCode.STRING)
                               .attributeTagIds(List.of())
                               .attributeRoleRelationIds(List.of())
                               .build();

client.createAttribute(attribute);
```

3. 属性の照会

```java
String attributeId = "";

Attribute attribute = client.getAttribute(attributeId);
```

4. 属性リストの照会

⚠️ レスポンス時に使われるモデルは`Common`を参照

**[GetAttributesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeIds|   List&lt;String> |**No**|  条件属性IDリスト |
|attributeIdPreLike|     String          |**No**|  条件属性ID(前方一致) |
|roleIds|    List&lt;String> |**No**| ロールIDリスト      |
|roleIdPreLike|  String          |**No**|  ロールID(前方一致)           |
|attributeTagIds|    List&lt;String> |**No**|  条件属性IDリスト |
|descriptionLike|    String          |**No**|  説明(部分一致)           |
|attributeDataType | AttributeDataType |**No**|  条件属性データタイプ  |

```java
GetAttributesRequest request = GetAttributesRequest.builder()
                                                  .attributeIds(List.Of())
                                                  .roleIds(List.Of())
                                                  .attributeTagIds(List.Of())
                                                  .attributeDataTypeCode(AttributeDataTypeCode.STRING)
                                                  .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<GetAttributeResponse> attributes = client.getAttributes(request, pageable);
```

**[GetAttributeResponse]**

| Key                           |   Type | Required |   Description   |
|--------------|----------------|----|----------|
| attribute                     |   Attribute                             |**Yes**| 条件属性モデル         |
| attributeTagById              |   Map&lt;String, AttributeTag>          |**No**| 条件属性タグ情報      |
| attributeRoleRelationByRoleId |   Map&lt;String, AttributeRoleRelation> |**No**| 条件属性と関連するロール |
| attributeInUse                |   Boolean                               |**Yes**| 条件属性データ型   |

5. 属性の修正

   ⚠️ リクエスト時に使用されるモデルは`Attribute`を参照

```java
Attribute attribute = Attribute.build()
                              .attributeId("")
                              .attributeName("")
                              .description("")
                              .build();

client.updateAttribute(attribute);
```

6. 属性の削除

**[DeleteAttributeRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String         |**Yes**|    条件属性ID |
|forceDelete|    boolean        |**No**|    強制削除を行うかどうか(デフォルト値: false) |

```java
DeleteAttributeRequest request = DeleteAttributeRequest.build()
                                                      .attributeId("")
                                                      .forceDelete(false)
                                                      .build();

client.deleteAttribute(request);
```

7. 属性一括削除

**[DeleteAttributesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeIds| Set&lt;String>      |**Yes**|  条件属性IDリスト |
|forceDelete|    boolean        |**No**|    強制削除を行うかどうか(デフォルト値: false) |

```java
DeleteAttributesRequest request = DeleteAttributesRequest.builder()
                                                          .operationIds(Set.of(""))
                                                          .build();

client.deleteAttributes(request);
```

#### 4. スコープ
> スコープ情報の登録、照会、修正、削除

1. Model

**[Scope]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String          |**Yes**| スコープID|
|description|    String      |**No**| 説明|

2. スコープの作成

```java
Scope scope = Scope.builder()
                   .scopeId("")
                   .description("")
                   .build();

client.createScope(scope);
```

3. スコープの照会

⚠️レスポンス時に使用されるモデルは`Model`を参照
```java
String scopeId = "";

Scope scope = client.getScope(scopeId);
```

4. スコープリストの照会

⚠️レスポンス時に使用されるモデルは`Common`を参照

**[GetScopesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeIds|   List&lt;String> |**No**|  スコープIDリスト |
|scopeIdPreLike|     String          |**No**|  スコープID(前方一致) |
|descriptionLike|    String          |**No**|  説明(部分一致)           |

```java
GetScopesRequest request = GetScopesRequest.builder()
                                          .scopeIdPreLike("")
                                          .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<Scope> scopes = client.getScopes(request, pageable);
```

5. スコープの修正

⚠️リクエスト時に使用されるモデルは`Scope`を参照

```java
Scope scope = Scope.builder()
                  .scopeId("")
                  .description("")
                  .build();

client.updateScope(scope);
```

6. スコープの削除

```java
String scopeId = "";

client.deleteScope(userId);
```

7. スコープの一括削除

**[DeleteScopesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeIds| Set&lt;String>      |**Yes**|  スコープIDリスト |

```java
DeleteScopesRequest request = DeleteScopesRequest.builder()
                                                 .scopeIds(Set.of(""))
                                                 .build();

client.deleteScopes(request);
```

#### 5. ロール
> ロール情報登録、照会、修正、削除及び登録されたロールの設定可能なAttributeリストの照会、DENY(未使用)に変更可能かどうか

1. Model

**[Role]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleMetaData|   RoleMetaData          |**Yes**|    ロール         |
|roleRelations|  List&lt;RoleRelation>    |**No**|  関連関係ロールIDリスト |
|roleTags|   List&lt;RoleTag> |**No**| ロールタグリスト       |

**[RoleMetaData]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleId|     String          |**Yes**|  ロールID          |
|roleName|   String          |**No**|  ロール名         |
|roleGroup|  String |**No**|   ロールグループ |
|description|    String   |**No**| 説明             |
|exposureOrder|  Integer   |**No**|    表示順序      |

**[RoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|relatedRoleId|  String          |**Yes**|  ロール関連関係ID          |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|  ロール使用有無: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**| ロール条件属性             |

**[Condition]**

|Key|    Type | Required |   Description   |
  |--------------|----------------|----|----------|
|attributeId| String |**Yes**|  条件属性ID          |
|attributeOperatorType | Required |**Yes**|  条件属性演算子タイプ  |
|attributeValues| List&lt;String> |**No**| 条件属性値          |

2. ロールの作成

```java
Role role = Role.builder()
                 .roleMetaData(RoleMetaData.build("")
                                           .roleId("")
                                           .roleName("")
                                           .roleGroup("")
                                           .description("")
                                           .exposureOrder(0)
                                           .build())
                 .roleRelations(RoleRelation.build()
                                            .relatedRoleId("")
                                            .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                            .conditions(Condition.build()
                                                                 .attributeId("")
                                                                 .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                 .attributeValues(List.of())
                                                                 .build())
                                            .build())
                 .roleTags(List.of())
                 .build();

client.createRole(role);
```

3. ロールの照会

⚠️レスポンス時に使用されるモデルは`Model`を参照

```java
String roleId = "";

Role role = client.getRole(roleId);
```

4. ロールリストの照会

**[GetRoleRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleIds              |    List&lt;String>               |**No**| ロールIDリスト(完全一致)                |
| roleIdPreLike        |    String                        |**No**| スコープID(前方一致)                  |
| relatedRoleIds       |    List&lt;String>               |**No**|  関連関係ロールIDリスト(完全一致)           |
| descriptionLike      |    String                        |**No**| 説明(部分一致)                        |
| roleNameLike         |    String                        |**No**| ロール名(部分一致)                   |
| roleGroup            |    String                        |**No**| ロールグループ(完全一致)                   |
| roleGroupLike        |    String                        |**No**| ロールグループ(部分一致)                   |
| roleTagIdExpr        |    String                        |**No**| ロールタグ条件(セパレータ';':OR, ',':AND) |
| roleTagIds           |    List&lt;String>               |**No**| ロールタグIDリスト(完全一致)            |
| attributeIds         |    List&lt;String>               |**No**| 条件属性IDリスト(完全一致)           |
| attributeTagIds      |    List&lt;String>               |**No**| 条件属性タグIDリスト(完全一致)       |
| needAttributes       |    Boolean                       |**No**| レスポンス時に条件属性情報を含めるかどうか           |
| needRoleTags         |    Boolean                       |**No**| レスポンス時にロールタグIDリストを含めるかどうか         |
| needRoleRelations    |    Boolean                       |**No**| レスポンス時に関連関係ロールIDリストを含めるかどうか        |
| searchRoleOptionCode |    SearchRoleOptionCode          |**No**| ロール検索時に下位ロールを含めるかどうか       |

```java
GetRoleRequest request = GetRoleRequest.builder()
                                      .roleIds("")
                                      .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<Role> roles = client.getRoles(request, pageable);
```

5. ロールの修正

⚠️リクエスト時に使用されるモデルは`Role`参考を参照

```java
Role role = Role.builder()
                 .roleMetaData(RoleMetaData.build()
                                           .roleId("")
                                           .roleName("")
                                           .roleGroup("")
                                           .description""()
                                           .exposureOrder(0)
                                           .build())
                 .roleRelations(RoleRelation.build()
                                            .relatedRoleId("")
                                            .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                            .conditions(Condition.build()
                                                                 .attributeId("")
                                                                 .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                 .attributeValues(List.of())
                                                                 .build())
                                            .build())
                 .roleTags(List.of())
                 .build();

client.updateRole(role);
```

6. ロールの削除

```java
String roleId = "";

client.deleteRole(roleId);
```

7. ロールの一括削除

**[DeleteRolesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleIds| Set&lt;String>      |**Yes**|  ロールIDリスト |

```java
DeleteRolesRequest request = DeleteRolesRequest.builder()
                                               .roleIds(Set.of(""))
                                               .build();

client.deleteRoles(request);
```

8. ロールで設定可能なすべての属性リストの照会

**[GetRoleAttributesRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId            |    String          |**Yes**|  ロールID                     |
| attributeIds      |    List&lt;String> |**No**|  条件属性IDリスト(完全一致)     |
| attributeTagIds   |    List&lt;String> |**No**|  条件属性タグIDリスト(完全一致) |
| attributeNameLike |    Boolean         |**No**|  レスポンス時の条件属性名(部分一致)   |

```java
GetRoleAttributesRequest request = GetRoleAttributesRequest.builder()
                                                          .roleId("")
                                                          .attributeIds(List.of())
                                                          .attributeTagIds(List.of())
                                                          .attributeNameLike(List.of()) 
                                                          .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<Attribute> attributes = client.getRoleAttributes(request, pageable);
```
⚠️レスポンス時に使用されるモデルは`3. 属性` Modelを参照

8. ロール使用有無をDENY(未使用)に変更できるかどうか

```java
String roleId = "";

boolean result = client.isDeniable(roleId);
```

#### 6. ロール関連関係
> ロール関連関係の登録、修正、削除

1. ロール関連関係の登録

**[CreateRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|  ロールID                       |
| roleRelations   |    List&lt;RoleRelation>  |**No**|   ⚠️ `5. ロール`のRoleRelation Modelを参照  |

```java
CreateRoleRelationRequest role = CreateRoleRelationRequest.builder()
                                                          .roleId("")
                                                          .roleRelations(RoleRelation.build()
                                                                                     .relatedRoleId("")
                                                                                     .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                                                                     .conditions(Condition.build()
                                                                                                          .attributeId("")
                                                                                                          .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                                                          .attributeValues(List.of())
                                                                                                          .build())
                                                                                     .build())
                                                          .build();

client.createRoleRelations(role);
```

2. ロール関連関係の修正

**[UpdateRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|  ロールID                       |
| roleRelations   |    List&lt;RoleRelation>  |**No**|   ⚠️ `5. ロール`のRoleRelation Modelを参照  |

```java
UpdateRoleRelationRequest role = UpdateRoleRelationRequest.builder()
                                                          .roleId("")
                                                          .roleRelations(RoleRelation.build()
                                                                                     .relatedRoleId("")
                                                                                     .roleApplyPolicyCode(RoleApplyPolicyCode.ALLOW)
                                                                                     .conditions(Condition.build()
                                                                                                          .attributeId("")
                                                                                                          .attributeOperatorTypeCode(AttributeOperatorTypeCode.STRING)
                                                                                                          .attributeValues(List.of())
                                                                                                          .build())
                                                                                     .build())
                                                          .build();

client.updateRoleRelation(role);
```

3. ロール関連関係の削除

**[DeleteRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|  ロールID                       |
| relatedRoleIds   |    List&lt;String>  |**No**|   関連ロールIDリスト  |

```java
DeleteRoleRelationRequest role = DeleteRoleRelationRequest.builder()
                                                          .roleId("")
                                                          .relatedRoleIds(List.Of(""))
                                                          .build();

client.deleteRoleRelations(role);
```

#### 7. リソース
> リソース情報の登録、照会、修正、削除

1. Model

**[Resource]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId    |    String  |**No**|  リソースID                       |
| description   |    String  |**No**|  説明                               |
| name          |    String  |**No**|  リソース名                      |
| path          |    String  |**Yes**|  リソースパス                    |
| uiPath        |    String  |**Yes**|  リソースUIパス                 |
| priority      |    Integer |**Yes**|  優先順位                             |
| metadata      |    String  |**No**|  メタデータ                            |
| newResourceId |    String  |**No**|  既に作成されたリソースIDをアップデートしたい場合にのみ使用 |

2. リソースの作成

```java
Resource resource = Resource.builder()
                            .resourceId("")
                            .description("")
                            .name("")
                            .path("")
                            .uiPath("")
                            .priority(0)
                            .metadata("")
                            .build();

client.createResource(resource);
```

3. リソースの照会

⚠️レスポンス時に使用されるモデルは`Model`を参照

```java
String resourceId = "";

Resource resource = client.getResource(resourceId);
```

4. リソースリストの照会

⚠️レスポンス時に使用されるモデルは`Common`を参照

**[GetResourcesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceIdPreLike    |     String               |**No**| リソースID(前方一致)      |
| resourcePath         |     String               |**No**| リソースパス(完全一致)    |
| resourcePathLike     |     String               |**No**| リソースパス(前方一致)         |
| resourceUiPath       |     String               |**No**| リソースUIパス(前方一致)      |
| resourceIds          |     List&lt;String>      |**No**| リソースIDリスト               |
| resourcePaths        |     List&lt;String>      |**No**| リソースパスリスト(完全一致)      |
| resourceUiPaths      |     List&lt;String>      |**No**| リソースUIパスリスト(完全一致)   |
| userIds              |     List&lt;String>      |**No**| リソースにアクセス可能なユーザーIDリスト   |
| scopeIds             |     List&lt;String>      |**No**| リソースにアクセス可能なスコープIDリスト    |
| roleIds              |     List&lt;String>      |**No**| リソースに付与されたロールIDリスト       |
| operationIds         |     List&lt;String>      |**No**| リソースに付与されたオペレーションIDリスト |
| searchRoleOptionCode |     SearchRoleOptionCode |**No**| ロール検索時に下位ロールを含めるかどうか      |

```java
GetResourcesRequest request = GetResourcesRequest.builder()
                                                .resourceIdPreLike("")
                                                .resourcePathLike("")
                                                .userIds(List.of())
                                                .searchRoleOptionCode(SearchRoleOptionCode.DIRECT)
                                                .build();
Pageable pageable = Pageable.builder()
                           .page(1)
                           .itemsPerPage(10)
                           .build();

Page<Resource> resources = client.getResources(request, pageable);
```

5. リソースの修正

⚠️リクエスト時に使用されるモデルは`Resource`を参照

```java
Resource resource = Resource.builder()
                           .resourceId("")
                           .description("")
                           .name("")
                           .path("")
                           .uiPath("")
                           .priority(0)
                           .metadata("")
                           .build();

client.updateResource(operation);
```

6. リソースの削除

```java
String resourceId = "";

client.deleteResource(resourceId);
```

7. リソースの一括削除

**[DeleteResourcesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|resourceIds| Set&lt;String>      |**Yes**|  リソースIDリスト |

```java
DeleteResourcesRequest request = DeleteResourcesRequest.builder()
                                                       .roleIds(Set.of(""))
                                                       .build();

client.deleteResources(request);
```

#### 8. リソースの階層構造
> リソースの階層構造を照会します。
> uiPath(resourceUiPath)を基準に階層構造が形成され、ユーザーが定義したキャッシュ時間だけキャッシュされます。

1. リソースの階層構造照会

**[GetResourceHierarchyRequest]**

| Key             | Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds         | List&lt;String> |**No**| ユーザーIDリスト  |
| roleIds         | List&lt;String> |**No**| ロールIDリスト   |
| operationIds    | List&lt;String> |**No**| オペレーションIDリスト |
| scopeIds        | List&lt;String> |**No**| スコープIDリスト   |
| resourceIds     | List&lt;String> |**No**| リソースIDリスト  |
| resourcePath    | String          |**No**| リソースパス     |
| resourceUiPath  | String          |**No**| リソースUIパス  |

```java
GetResourceHierarchyRequest request = GetResourceHierarchyRequest.builder()
                                                                .resourceIds(List.of(""))
                                                                .build());

List<ResourceHierarchy> responses = client.getResourceHierarchy(request);
```

**[ResourceHierarchy]**

| Key         | Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId  | String                     |**Yes**| リソースID                                  |
| description | String                     |**No**| リソースの説明                                 |
| name        | String                     |**Yes**| リソース名                                 |
| path        | String                     |**Yes**| リソースパス                                 |
| uiPath      | String                     |**Yes**| リソースUIパス<br/>このパスに基づいて階層構造が作成されます。 |
| priority    | Integer                    |**Yes**| 優先順位                                   |
| resources   | List&lt;ResourceHierarchy> |**No**| 下位リソース                                 |

#### 9. ユーザー認可(user authorization)
> ユーザーが特定のロールを持っているか、リソースに対するアクセス権を持っているかどうかを確認します。
> リソースの場合、ユーザーが定義したキャッシュ時間だけキャッシュされます。

1.特定のリソースの認可結果確認

* リクエストする際、リソースIDまたはリソースパスのいずれかが必ず必要です。
* リソースIDとリソースパスの両方の値を入れると、リソースIDを優先して検査するため、リソースパスで検査したい場合は、リソースIDを設定しないでください。

**[GetResourceAuthorizationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId | String                          |**No**| ユーザーが定義したID値<br/>どの認証条件に対するレスポンスであるかを明確に知る必要がある場合に使用します。 |
| operationId   | String                          |**Yes**| オペレーションID                                              |
| resourceId    | String                          |**No**| リソースID(リソースパスがない場合は必須)                             |
| resourcePath  | String                          |**No**| リソースパス(リソースIDがない場合は必須)                       |
| scopeId       | String                          |**No**| スコープID                                                  |
| attributes    | List&lt;AuthorizationAttribute> |**No**| 条件属性リスト                                                    |

**[AuthorizationAttribute]**

| Key            |   Type | Required |   Description   |
|--------------|----------------|----|----------|
| attributeId    | String                           |**Yes**| 条件属性ID       |
| attributeValue | String                           |**Yes**| 条件属性値       |

```java
String userId = "userId";
GetResourceAuthorizationRequest request = GetResourceAuthorizationRequest.builder()
                                                                        .operationId("")
                                                                        .resourceId("")
                                                                        .scopeId("")
                                                                        .attributes(List.of(AuthorizationAttribute.builder()
                                                                                                                    .attributeId("")
                                                                                                                    .attributeValue("")
                                                                                                                    .build()));
                                                                        .build();

boolean response = client.hasAuthorizationByResource(userId, request);
```

2. 複数のリソースの認可結果確認
* リクエストした順番でレスポンスが返されます。

```java
String userId = "userId";
List<GetResourceAuthorizationRequest> requests = requests = List.of(GetResourceAuthorizationRequest.builder()
                                                                                                    .authRequestId("")
                                                                                                    .operationId("")
                                                                                                    .resourceId("")
                                                                                                    .scopeId("")
                                                                                                    .build(),
                                                                    GetResourceAuthorizationRequest.builder()
                                                                                                    .authRequestId("")
                                                                                                    .operationId("")
                                                                                                    .resourcePath("")
                                                                                                    .build());

List<GetResourceAuthorizationResponse> responses = client.hasAuthorizationByResources(userId, requests);
```

**[GetResourceAuthorizationResponse]**

| Key           |    Type | Required | Description                                 |
|--------------|----------------|----|---------------------------------------------|
| authRequestId | String                          |**No**| ユーザーが定義したID値<br/>リクエスト時に送った値がそのまま返されます。     |
| operationId   | String                          |**Yes**| オペレーションID                                    |
| resourceId    | String                          |**Yes**| リソースID                                      |
| resourcePath  | String                          |**No**| リソースパス                                     |
| scopeId       | String                          |**Yes**| スコープID                                       |
| permission    | Boolean                         |**Yes**| 認可結果<br/><br/>true:権限あり<br/>false:権限なし |
| attributes    | List&lt;AuthorizationAttribute> |**No**| 条件属性リスト                                   |

3. 特定のロールの認可結果確認

**[GetRoleAuthorizationRequest]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId  | String                          |**No**|  ユーザーが定義したID値<br/>どの認証条件に対する応答であるかを明確に知る必要がある場合に使用。 |
| attributes     | List&lt;AuthorizationAttribute> |**No**| 条件属性リスト                                                    |
| roleId         | String                          |**Yes**| ロールID                                                     |
| scopeId        | String                          |**No**| スコープID                                                  |

```java
String userId = "userId";
GetRoleAuthorizationRequest getUserRequest = GetRoleAuthorizationRequest.builder()
                                                                        .authRequestId("")
                                                                        .roleId("")
                                                                        .scopeId("")
                                                                        .attributes(List.of(AuthorizationAttribute.builder()
                                                                                                                    .attributeId("")
                                                                                                                    .attributeValue("")
                                                                                                                    .build()))
                                                                        .build(); 

boolean response = client.hasAuthorizationByRole(userId, request);
```

4. 複数のロールの認可結果確認

```java
String userId = "userId";
GetRoleAuthorizationRequest requests = List.of(GetRoleAuthorizationRequest.builder()
                                                                        .authRequestId("")
                                                                        .roleId("")
                                                                        .scopeId("")
                                                                        .attributes(List.of(AuthorizationAttribute.builder()
                                                                                                                    .attributeId("")
                                                                                                                    .attributeValue("")
                                                                                                                    .build()))
                                                                        .build(),
                                                GetRoleAuthorizationRequest.builder()
                                                                        .authRequestId("")
                                                                        .roleId("")
                                                                        .scopeId("")
                                                                        .attributes(List.of(AuthorizationAttribute.builder()
                                                                                                                    .attributeId("")
                                                                                                                    .attributeValue("")
                                                                                                                    .build()))
                                                                        .build());

List<GetRoleAuthorizationResponse> responses = client.hasAuthorizationByRoles(userId, requests);
```

**[GetRoleAuthorizationResponse]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId  | String                          |**No**| ユーザーが定義したID値<br/>リクエスト時に送った値がそのまま返されます。        |
| roleId         | String                          |**Yes**| ロールID                                         |
| scopeId        | String                          |**Yes**| スコープID
| permission     | Boolean                         |**Yes**| 認可結果<br/><br/>true:権限あり<br/>false:権限なし |
| attributes     | List&lt;AuthorizationAttribute> |**No**| 条件属性リスト                                     |

### クライアントSDKキャッシュ

クライアントSDKでは、下記の3つの場合に、それぞれクライアント側のキャッシュを使用します。

- Resource IDを利用した権限チェック
- Resource Pathを利用した権限チェック
- Resource Hierarchy照会

LRUで管理しており、キャッシュのデフォルト値は300秒のTTL(time to live)と1,000,000個Sizeです。
この値を修正するにはNHN Cloudコンソールに接続します。
NHN Cloudコンソールで変更した設定は、変更されるとすぐに反映され、変更されると同時に既存のキャッシュはすべて削除されます。

![[図2]クライアントSDKキャッシュ設定](http://static.toastoven.net/prod_role/role_62.png)
<center>[図2]クライアントSDKキャッシュ設定</center>

### Transactionサポート

ROLEのデータをAtomicに追加/変更/削除したい場合は、RoleClientオブジェクトのbeginTransaction()を呼び出してRoleSessionオブジェクトを取得して使います。

例えば、下記のように複数のRoleを同時に登録する際、途中でエラーが発生すると、いくつかは登録されて、いくつかは登録できない場合があります。

```java
RoleClient client = RoleClientFactory.getClient(RoleConfig.builder()
                                                           .appKey("")
                                                           .secretKey("")
                                                           .build());

try {
   User user = User.build()
                   .userId("U1")
                   .description("desc")
                   .build();

   client.userCreate(user);
   
   
   Role role = Role.builder()
                   .roleMetaData(RoleMetaData.build()
                                             .roleId("M1")
                                             .roleName("MEMBER")
                                             .exposureOrder(1)
                                             .build())
                   .build();

	// もし、ここでExceptionが発生したら
	// U1は作成されますが、M1は作成されません。
	client.createRole(role);
} catch (Exception e) {
    // エラー発生時、独自にRollbackロジックを実装する必要があります。
    client.userDelete("U1");
}
```

RoleSessionオブジェクトを使うと、上のような状況で部分的な失敗をなくすことができます。

```java
RoleClient client = RoleClientFactory.getClient(RoleConfig.builder()
                                                           .appKey("")
                                                           .secretKey("")
                                                           .build());
RoleSession session = client.beginTransaction();

try {
   User user = User.build()
                   .userId("U1")
                   .description("desc")
                   .build();

   session.userCreate(user);
   
   
   Role role = Role.builder()
                   .roleMetaData(RoleMetaData.build()
                                             .roleId("M1")
                                             .roleName("MEMBER")
                                             .exposureOrder(1)
                                             .build())
                   .build();

   // もし、ここでExceptionが発生しても、部分的な失敗は発生しません。
   session.createRole(role);
   // エラーが発生しなかった場合、サーバーに変更事項を反映します。
   session.commit();
} catch (Exception e) {
   // エラーが発生した時、rollback関数を使ってsessionに保存された変更内容を空にします。
   session.rollback();
}
```

RoleSessionオブジェクトを使う時、commit()メソッドを呼び出す前までは追加/修正/変更がサーバーに反映されないので、commit()する前に変更したデータを読まないように注意する必要があります。

RoleSessionオブジェクトをcommit()したりrollback()した後、再使用できます。

> RoleSessionは`SDK使用ガイド`で定義されたサービスのうち、照会を除いた登録、修正、削除に対して同じように使用できます。
