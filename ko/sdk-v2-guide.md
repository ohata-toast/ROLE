## Application Service > ROLE > SDK 사용 가이드

> Role 상품을 이용하여 권한을 체크하기 위해서는
> RESTFUL API 를 호출하거나, Client SDK 를 이용하여야 한다.

## AppKey & SecretKey

RESTFUL API 와 Client SDK 를 사용하려면 AppKey 와 Secret Key 가 필요하다.
[CONSOLE] 의 좌측 상단에서 발급된 Key 정보를 확인 할 수 있다.

![[그림 1] AppKey & SecretKey 확인](http://static.toastoven.net/prod_role/role_60.png)
<center>[그림 1] AppKey & SecretKey 확인</center>

## Client SDK

### Client SDK 란?

RESTFUL API를 손쉽게 호출하기 위한 Role 전용 Client SDK 이다.
자체 Cache 기능을 가지고 있기 때문에, 좀더 효율적으로 Role 상품을 이용 할 수 있다.
현재는 JAVA 언어에 대해서만 지원을 하고 있다.

### 사용 환경
`JDK 11` 버전 이상의 환경

### Maven 을 이용한 JAVA Client SDK 사용

JAVA Client SDK 를 사용하기 위해선 pom.xml 에 maven repository 및 depencency 설정이 필요하다.

**[Maven Repository]**
Maven Central Repository 에 저장되어 있어 별도의 설정은 필요 없음.
만약 다른 저장소를 사용하거나 Maven Central이 참조되지 않는 환경에서는 다음과 같이 설정.

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

### JAVA Client SDK 사용법

JAVA Client SDK 를 사용하기 위해선 먼저 RoleClientFactory 객체를 이용하여 RoleClient 객체의 instance 를 생성해야 한다.
RoleClient 객체를 생성하였으면, 해당 객체에서 제공하는 method 를 호출하여 여러 작업들을 처리하면 된다.

**[RoleConfig]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| appKey         | String  |**Yes**| 서버에서 발급받은 앱키                                                       |
| secretKey      | String  |**Yes**| 서버에서 발급받은 비밀 키                                                     |
| domain         | String  |**No**| 도메인 주소<br/>기본으로 설정된 값을 사용하면 되며, 별도로 설정할 필요는 없다                     |
| connectTimeout | Integer |**No**| 연결 타임아웃을 설정할 수 있으며, 시간단위는 밀리세컨드이다.<br/>기본값은 okHttp 의 기본값인 10초이다.   |
| readTimeout    | Integer |**No**| Read 타임아웃을 설정할 수 있으며, 시간단위는 밀리세컨드이다.<br/>기본값은 okHttp 의 기본값인 10초이다. |

```java
String appKey = "appKey";
String secretKey = "secretKey";

// RoleClient 객체를 생성하는 올바른 방법
// domain 은 별도로 설정해주지 않아도 된다
RoleClient client = RoleClientFactory.getClient(RoleConfig.builder()
                                                            .appKey(appKey)
                                                            .secretKey(secretKey)
                                                            .connectTimeout(30_000)
                                                            .readTimeout(60_000)
                                                            .build());

// 아래처럼 직접 생성자를 호출하면 안된다.
RoleClient client = new RoleClient(RoleConfig.builder()
                                                .appKey(appKey)
                                                .secretKey(secretKey)
                                                .connectTimeout(30_000)
                                                .readTimeout(60_000)
                                                .build());
```

> RoleClient 의 생성자를 직접 호출하지 않도록 주의한다.

### SDK 사용 가이드
#### Common
> SDK를 사용함에 기능에서 공통으로 사용되는 부분

1. 페이징을 위한 요청/응답 모델

**[Pageable]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| page         | Integer| **No** | 페이지 번호         |
| itemsPerPage | Integer | **No** | 목록 당 조회될 아이템 수 |
| sort              | List&lt;String>      | **No** |    데이터 정렬 기준     |

**[Page]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| totalItems         | Integer    | **Yes** | 전체 개수    |
| items | List&lt;T> | **Yes** | 조회된 목록     |

#### 1. User
> 사용자 정보 등록, 조회, 수정, 삭제 기능 및 사용자 역할 변경 내역 조회

1. Model

**[User]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|userId|     String |**Yes**| 사용자 ID|
|description|    String  |**No**| 설명|
|roleRelations|  List&lt;UserRoleRelation> |**No**| 연관 역할|

**[UserRoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String            |**Yes**|     적용 대상 ID              |
|roleId|     String            |**Yes**|     역할 ID                 |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|   역할 사용 여부: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**|  역할 조건 속성              |

**[Condition]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId| String |**Yes**|   조건 속성 ID          |
|attributeOperatorType | Required |**Yes**|   Description   |
|attributeValues| List&lt;String> |**No**|  조건 속성 값           |

2. User 생성

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

3. User 조회

**[GetUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**Yes**|    사용자 ID                                             |
| searchRoleOptionCode | SearchRoleOptionCode    |**No**|   연관관계 권한을 포함해서 검색할 지 여부: DIRECT_ROLE, INDIRECT_ROLE |
| roleIds              | List&lt;String> |**No**|   역할 ID 목록      |
| scopeIds             | List&lt;String> |**No**|   적용 대상 ID 목록 |

```java
GetUserRequest request = GetUserRequest.builder()
                                       .userId("")
                                       .build();

User user = client.getUser(request);
```

4. User 목록 조회

⚠️ 응답 시 사용되는 모델은 `Common` 참고

**[GetUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds              | List&lt;String>      |**No**|   사용자 ID 목록 (완전 일치) |
| userIdPreLike        | String               |**No**|   사용자 ID (전방 일치) |
| scopeIds             | List&lt;String>      |**No**|   범위 ID 목록 (완전 일치) |
| scopeIdPreLike       | String               |**No**|   범위 ID (전방 일치) |
| roleIds              | List&lt;String>      |**No**|   역할 ID 목록 (완전 일치) |
| roleIdPreLike        | String               |**No**|   역할 ID (전방 일치) |
| descriptionLike      | String               |**No**|   사용자 설명 (부분 일치) |
| searchRoleOptionCode | SearchRoleOptionCode |**No**|   접근 가능한 역할 목록 검색 방식 |
| needRoleRelations    | Boolean              |**No**|   응답 시 역할 연관관계 포함 여부 |
| needRoleTags         | Boolean              |**No**|   응답 시 역할 연관관계 포함 시 역할 태그 포함 여부 |

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

5. User 수정

⚠️ 요청 시 사용되는 모델은 `User` 참고

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

client.updateUser(user);
```

6. User 삭제

```java
String userId = "";

client.deleteUser(userId);
```

7. User 역할 변경 내역 리스트 조회

**[GetUserRoleHistoriesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**No**|    사용자 ID      |
| roleId              | String |**No**|     역할 ID       |
| scopeId             | String   |**No**| 적용 대상 ID
| fromDateTime | OffsetDateTime   |**No**|  변경 시작일시     |
| toDateTime | OffsetDateTime   |**No**|    변경 종료일시     |

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
| userHistorySeq               | long           |**Yes**|   순번           |
| userId               | String         |**Yes**|   사용자 ID      |
| roleId              | String         |**No**|    역할 ID       |
| scopeId             | String          |**No**| 적용 대상 ID
| roleApplyPolicyCode | RoleApplyPolicyCode |**No**|   역할 사용 여부: ALLOW, DENY     |
| conditions | List&lt;ConditionBundle> |**No**|   역할 조건 속성     |
| command | UserRoleHistoryCommandCode |**Yes**|    명령     |
| executionTime | OffsetDateTime |**Yes**|  변경 일시     |
| operatorUuid | String |**Yes**|   작업자 UUID     |

#### 2. Operation
> Operation 정보 등록, 조회, 수정, 삭제

1. Model

**[Operation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationId|    String                    |**Yes**| 오퍼레이션 ID|
|description|    String                    |**No**| 설명|

2. Operation 생성

```java
Operation operation = Operation.builder()
                               .operationId("")
                               .description("")
                               .build();

client.createOperation(operation);
```

3. Operation 조회

⚠️ 응답 시 사용되는 모델은 `Model` 참고

```java
String operationId = "";

Operation operation = client.getOperation(operationId);
```

4. Operation 목록 조회

⚠️ 응답 시 사용되는 모델은 `Common` 참고

**[GetOperationsRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationIds|   List&lt;String> |**No**|   오퍼레이션 ID 목록 |
|operationIdPreLike|     String          |**No**|   오퍼레이션 ID (전방 일치) |
|descriptionLike|    String          |**No**|   설명 (부분 일치)           |

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

5. Operation 수정

⚠️ 요청 시 사용되는 모델은 `Operation` 참고
```java
Operation operation = Operation.builder()
                .operationId("")
                .description("")
                .build();

client.updateOperation(operation);
```

6. Operation 삭제

```java
String operationId = "";

client.deleteOperation(userId);
```

#### 3. Attribute
> attribute 정보 등록, 조회, 수정, 삭제

1. Model

**[Attribute]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String          |**Yes**|   조건 속성 ID                                 |
|attributeName|  String          |**No**|   조건 속성 이름                                 |
|description|    String          |**No**|   설명                                           |
|attributeDataType | AttributeDataType |**Yes**|   Description   |
|attributeCreationType | AttributeCreationType |**No**|   Description   |
|attributeTagIds|    List&lt;String>    |**No**|    조건 속성 태그 목록                              |
|attributeRoleRelationIds|   List&lt;String> |**No**|   연관 역할 목록                                     |

2. Attribute 생성

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

3. Attribute 조회

```java
String attributeId = "";

Attribute attribute = client.getAttribute(attributeId);
```

4. Attribute 목록 조회

⚠️ 응답 시 사용되는 모델은 `Common` 참고

**[GetAttributesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeIds|   List&lt;String> |**No**|   조건 속성 ID 목록 |
|attributeIdPreLike|     String          |**No**|   조건 속성 ID (전방 일치) |
|roleIds|    List&lt;String> |**No**| 역 ID 목록       |
|roleIdPreLike|  String          |**No**|   역할 ID (전방 일치)           |
|attributeTagIds|    List&lt;String> |**No**|   조건 속성 ID 목록 |
|descriptionLike|    String          |**No**|   설명 (부분 일치)           |
|attributeDataType | AttributeDataType |**No**|   Description   |

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
| attribute                     |   Attribute                             |**Yes**| 조건 속성 모델          |
| attributeTagById              |   Map&lt;String, AttributeTag>          |**No**| 조건 속성 태그 정보       |
| attributeRoleRelationByRoleId |   Map&lt;String, AttributeRoleRelation> |**No**| 조건 속성과 연관된 역할 |
| attributeInUse                |   Boolean                               |**Yes**| 조건 속성 데이터 타입    |

5. Attribute 수정

   ⚠️ 요청 시 사용되는 모델은 `Attribute` 참고

```java
Attribute attribute = Attribute.build()
                              .attributeId("")
                              .attributeName("")
                              .description("")
                              .build();

client.updateAttribute(attribute);
```

6. Attribute 삭제

**[RemoveAttributeRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String         |**Yes**|     조건 속성 ID |
|forceDelete|    boolean        |**No**|     강제 삭제 여부(기본값: false) |

```java
RemoveAttributeRequest request = RemoveAttributeRequest.build()
                                                      .attributeId("")
                                                      .forceDelete(false)
                                                      .build();

client.deleteAttribute(userId);
```

#### 3. Scope
> Scope 정보 등록, 조회, 수정, 삭제

1. Model

**[Scope]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String          |**Yes**| 범위 ID|
|description|    String      |**No**| 설명|

2. Scope 생성

```java
Scope scope = Scope.builder()
                   .scopeId("")
                   .description("")
                   .build();

client.createScope(scope);
```
3. Scope 조회

⚠️ 응답 시 사용되는 모델은 `Model` 참고
```java
String scopeId = "";

Scope scope = client.getScope(scopeId);
```
4. Scope 목록 조회

⚠️ 응답 시 사용되는 모델은 `Common` 참고

**[GetScopesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeIds|   List&lt;String> |**No**|   Scope ID 목록 |
|scopeIdPreLike|     String          |**No**|   범위 ID (전방 일치) |
|descriptionLike|    String          |**No**|   설명 (부분 일치)           |

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

5. Scope 수정

⚠️ 요청 시 사용되는 모델은 `Scope` 참고

```java
Scope scope = Scope.builder()
                  .scopeId("")
                  .description("")
                  .build();

client.updateScope(scope);
```

6. Scope 삭제

```java
String scopeId = "";

client.deleteScope(userId);
```

#### 4. Role
> Role 정보 등록, 조회, 수정, 삭제 및 등록된 Role의 설정 가능한 Attribute 목록 조회, DENY(미사용)로 변경가능 여부

1. Model

**[Role]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleMetaData|   RoleMetaData          |**Yes**|     역할          |
|roleRelations|  List&lt;RoleRelation>    |**No**|  연관관계 역할 ID 목록 |
|roleTags|   List&lt;RoleTag> |**No**|  역할 태그 목록        |

**[RoleMetaData]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleId|     String          |**Yes**|   역할 ID          |
|roleName|   String          |**No**|   역할 이름          |
|roleGroup|  String |**No**|    역할 그룹 |
|description|    String   |**No**|  설명              |
|exposureOrder|  Integer   |**No**|     노출 순서       |

**[RoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|relatedRoleId|  String          |**Yes**|   역할 연관관계 ID          |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|   역할 사용 여부: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**|  역할 조건 속성              |

**[Condition]**

|Key|    Type | Required |   Description   |
  |--------------|----------------|----|----------|
|attributeId| String |**Yes**|   조건 속성 ID          |
|attributeOperatorType | Required |**Yes**|   Description   |
|attributeValues| List&lt;String> |**No**|  조건 속성 값           |

2. Role 생성

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

3. Role 조회

⚠️ 응답 시 사용되는 모델은 `Model` 참고

```java
String roleId = "";

Role role = client.getRole(roleId);
```

4. Role 목록 조회

**[GetRoleRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleIds           |    List&lt;String>  |**No**|  역할 ID 목록 (완전 일치)                |
| roleIdPreLike     |    String           |**No**|  범위 ID (전방 일치)                  |
| relatedRoleIds    |    List&lt;String>  |**No**|  연관관계 역할 ID 목록 (완전 일치)           |
| descriptionLike   |    String           |**No**|  설명 (부분 일치)                        |
| roleNameLike      |    String           |**No**|  역할 이름 (부분 일치)                   |
| roleGroupLike     |    String           |**No**|  역할 그룹 (부분 일치)                   |
| roleTagIdExpr     |    String           |**No**|  역할 태그 조건 (구분자 ';':OR, ',':AND) |
| roleTagIds        |    List&lt;String>  |**No**|  역할 태그 ID 목록 (완전 일치)            |
| attributeIds      |    List&lt;String>  |**No**|  조건 속성 ID 목록 (완전 일치)           |
| attributeTagIds   |    List&lt;String>  |**No**|  조건 속성 Tag ID 목록 (완전 일치)       |
| needAttributes    |    Boolean          |**No**|  응답 시 조건 속성 정보 포함 여부           |
| needRoleTags      |    Boolean          |**No**|  응답 시 역할 태그 ID 목록 포함 여부         |
| needRoleRelations |    Boolean          |**No**|  응답 시 연관관계 역할 ID 목록 포함 여부        |

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

5. Role 수정

⚠️ 요청 시 사용되는 모델은 `Role` 참고

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

6. Role 삭제

```java
String roleId = "";

client.deleteRole(roleId);
```

7. Role에서 설정 가능한 모든 Attribute 목록 조회

**[GetRoleAttributesRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId            |    String          |**Yes**|   역할 ID                     |
| attributeIds      |    List&lt;String> |**No**|   조건 속성 ID 목록 (완전 일치)     |
| attributeTagIds   |    List&lt;String> |**No**|   조건 속성 Tag ID 목록 (완전 일치) |
| attributeNameLike |    Boolean         |**No**|   응답 시 조건 속성 이름 (부분 일치)   |

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
⚠️ 응답 시 사용되는 모델은 `3. Attribute` Model 참고

8. 역할 사용여부 DENY(미사용)로 변경가능 여부

```java
String roleId = "";

boolean result = client.isDeniable(roleId);
```

#### 5. Resource
> Resource 정보 등록, 조회, 수정, 삭제

1. Model

**[Resource]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId    |    String  |**No**|   리소스 ID                       |
| description   |    String  |**No**|   설명                                |
| name          |    String  |**No**|   리소스 이름                       |
| path          |    String  |**Yes**|   리소스 Path                     |
| uiPath        |    String  |**Yes**|   리소스 UI Path                  |
| priority      |    Integer |**Yes**|   우선순위                              |
| metadata      |    String  |**No**|   메타데이터                             |
| newResourceId |    String  |**No**|   기존에 생성된 리소스의 ID 를 업데이트 하고 싶을때만 사용 |

2. Resource 생성

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

3. Resource 조회

⚠️ 응답 시 사용되는 모델은 `Model` 참고
```java
String resourceId = "";

Resource resource = client.getResource(resourceId);
```

4. Resource 목록 조회

⚠️ 응답 시 사용되는 모델은 `Common` 참고

**[GetResourcesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceIdPreLike    |     String               |**No**|  리소스 ID (전방 일치)      |
| resourcePath         |     String               |**No**|  리소스 Path (완전 일치)    |
| resourcePathLike     |     String               |**No**|  리소스 Path (전방 일치)         |
| resourceUiPath       |     String               |**No**|  리소스 UI Path (전방 일치)      |
| resourceIds          |     List&lt;String>      |**No**|  리소스 ID 목록                |
| resourcePaths        |     List&lt;String>      |**No**|  리소스 Path 목록 (완전 일치)      |
| resourceUiPaths      |     List&lt;String>      |**No**|  리소스 UI Path 목록 (완전 일치)   |
| userIds              |     List&lt;String>      |**No**|  리소스에 접근 가능한 사용자 ID 목록    |
| scopeIds             |     List&lt;String>      |**No**|  리소스에 접근 가능한 범위 ID 목록     |
| roleIds              |     List&lt;String>      |**No**|  리소스에 부여된 역할 ID 목록        |
| operationIds         |     List&lt;String>      |**No**|  리소스에 부여된 Operation ID 목록 |
| searchRoleOptionCode |     SearchRoleOptionCode |**No**|  역할 검색 시 하위 역할 포함 여부      |

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

5. Resource 수정

⚠️ 요청 시 사용되는 모델은 `Resource` 참고

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

6. Resource 삭제

```java
String resourceId = "";

client.deleteResource(resourceId);
```


#### 7. 리소스 계층구조
> 리소스의 계층구조를 조회한다.
> uiPath (resourceUiPath) 를 기준으로 계층구조가 형성되며, 사용자가 정의한 캐시 시간만큼 캐싱된다.

1. 리소스 계층 구조 조회

**[GetResourceHierarchyRequest]**

| Key             | Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds         | List&lt;String> |**No**| 사용자 ID 목록   |
| roleIds         | List&lt;String> |**No**| 역할 ID 목록    |
| operationIds    | List&lt;String> |**No**| 오퍼레이션 ID 목록 |
| scopeIds        | List&lt;String> |**No**| 범위 ID 목록    |
| resourceIds     | List&lt;String> |**No**| 리소스 ID 목록   |
| resourcePath    | String          |**No**| 리소스 경로      |
| resourceUiPath  | String          |**No**| 리소스 UI 경로   |

```java
GetResourceHierarchyRequest request = GetResourceHierarchyRequest.builder()
                                                                .resourceIds(List.of(""))
                                                                .build());

List<ResourceHierarchy> responses = client.getResourceHierarchy(request);
```

**[ResourceHierarchy]**

| Key         | Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId  | String                     |**Yes**| 리소스 ID                                  |
| description | String                     |**No**| 리소스 설명                                  |
| name        | String                     |**Yes**| 리소스 이름                                  |
| path        | String                     |**Yes**| 리소스 경로                                  |
| uiPath      | String                     |**Yes**| 리소스 UI 경로<br/>계층 구조가 이 경로를 기반으로 만들어 진다  |
| priority    | Integer                    |**Yes**| 우선 순위                                   |
| resources   | List&lt;ResourceHierarchy> |**No**| 하위 리소스들                                 |

#### 8. 사용자 인가 (User Authorization)
> 사용자가 특정한 역할을 가지고 있거나, 리소스에 대한 접근 권한을 가지고 있는지를 확인한다.
> 리소스의 경우 사용자가 정의한 캐시 시간 만큼 캐싱된다.

1. 특정 리소스의 인가 결과 확인

* 요청할 때 리소스 ID 나 리소스 경로 중 하나는 반드시 있어야 한다
* 리소스 ID 와 리소스 경로 값을 모두 넣으면 리소스 ID 를 우선하여 검사하기 때문에, 리소스 경로로 검사하고 싶은 경우 리소스 ID 를 설정하면 안된다

**[GetResourceAuthorizationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId | String                          |**No**| 사용자가 정의한 ID 값<br/>어떤 인증 조건에 대한 응답인지에 대해 명확하게 알아야 하는 경우 사용 |
| operationId   | String                          |**Yes**| 오퍼레이션 ID                                              |
| resourceId    | String                          |**No**| 리소스 ID(리소스 경로가 없으면 필수)                             |
| resourcePath  | String                          |**No**| 리소스 경로(리소스 ID 가 없으면 필수)                       |
| scopeId       | String                          |**No**| 범위 ID                                                  |
| attributes    | List&lt;AuthorizationAttribute> |**No**| 조건 속성 목록                                                     |

**[AuthorizationAttribute]**

| Key            |   Type | Required |   Description   |
|--------------|----------------|----|----------|
| attributeId    | String                           |**Yes**| 조건 속성 ID       |
| attributeValue | String                           |**Yes**| 조건 속성 값        |

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

2. 여러 리소스의 인가 결과 확인
* 요청한 순서대로 응답이 반환된다

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

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId | String                          |**No**| 사용자가 정의한 ID 값<br/>요청 시에 보낸 값이 그대로 반환된다       |
| operationId   | String                          |**Yes**| 오퍼레이션 ID                                 |
| resourceId    | String                          |**Yes**| 리소스 ID                                       |
| resourcePath  | String                          |**No**| 리소스 경로                                       |
| scopeId       | String                          |**Yes**| 범위 ID                                        |
| permission    | Boolean                         |**Yes**| 인가 결과<br/><br/>true : 권한 있음<br/>false : 권한 없음 |
| attributes    | List&lt;AuthorizationAttribute> |**No**| 조건 속성 목록                                     |

3. 특정 역할의 인가 결과 확인

**[GetRoleAuthorizationRequest]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId  | String                          |**No**| 사용자가 정의한 ID 값<br/>어떤 인증 조건에 대한 응답인지에 대해 명확하게 알아야 하는 경우 사용 |
| attributes     | List&lt;AuthorizationAttribute> |**No**| 조건 속성 목록                                                     |
| roleId         | String                          |**Yes**| 역할 ID                                                     |
| scopeId        | String                          |**No**| 범위 ID                                                  |

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

4. 여러 역할의 인가 결과 확인

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
| authRequestId  | String                          |**No**| 사용자가 정의한 ID 값<br/>요청 시에 보낸 값이 그대로 반환된다        |
| roleId         | String                          |**Yes**| 역할 ID                                         |
| scopeId        | String                          |**Yes**| 범위 ID
| permission     | Boolean                         |**Yes**| 인가 결과<br/><br/>true : 권한 있음<br/>false : 권한 없음 |
| attributes     | List&lt;AuthorizationAttribute> |**No**| 조건 속성 목록                                      |

### Client SDK Cache

Client SDK 에서는 아래 3가지 경우에 대해서 각각 Client 단의 Cache 를 사용한다.

- Resource ID 를 이용한 권한 체크
- Resource Path 를 이용한 권한 체크
- Resource Hierarchy 조회

LRU 로 관리를 하고 있으며, Cache 의 기본값은 300초의 TTL (Time To Live) 과 1,000,000 개 Size 이다.
해당 값을 수정 하려면 [CONSOLE] 에 접속하여 변경할 수 있다.
[CONSOLE] 에서 변경한 설정은 변경 즉시 반영되며, 변경되는 즉시 기존 Cache 는 모두 삭제된다.

![[그림 2] Client SDK Cache 설정](http://static.toastoven.net/prod_role/role_62.png)
<center>[그림 2] Client SDK Cache 설정</center>

### Transaction 지원

ROLE 의 데이터를 Atomic 하게 추가 / 변경 / 삭제 하고 싶을 경우에는 RoleClient 객체의 beginTransaction() 을 호출하여 RoleSession 객체를 얻어와 사용하면된다.

예를 들어, 아래와 같이 여러개의 Role 동시에 등록할 경우 중간에 에러 발생 시 몇개는 등록이 되고, 몇개는 등록이 안될 수 있다.

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

   // 만약 여기서 Exception 이 발생한다면
   // U1 은 생성되지만 M1은 생성되지 않는다.
   client.createRole(role);
} catch (Exception e) {
   // 에러시 자체 Rollback 로직을 구현해야 한다.
   client.userDelete("U1");
}
```

RoleSession 객체를 사용한다면, 위와 같은 상황에서 부분 실패를 없앨 수 있다.

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

   // 만약 여기서 Exception 이 발생한다 하여도, 부분 실패는 발생하지 않는다.
   session.createRole(role);
   // 에러가 발생하지 않았다면, 서버에 변경사항을 반영한다.
   session.commit();
} catch (Exception e) {
   // 에러 시, session에 저장된 변경 사항들을 rollback 함수를 통해 비운다
   session.rollback();
}
```

RoleSession 객체를 사용 시 commit() method 를 호출하기 전까지는 어떠한 추가 / 수정 / 변경사항도 서버에 반영되지 않기 때문에, commit() 하기 전 변경한 데이터를 읽지 않도록 주의해야한다.

RoleSession 객체를 commit() 하거나 rollback() 한 다음 다시 재사용 할 수 있다.

> RoleSession은`SDK 사용 가이드`에서 정의된 서비스 중 조회를 제외한 등록, 수정, 삭제에 대해서 동일하게 사용 가능하다.
