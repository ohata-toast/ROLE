## Application Service > ROLE > SDK user guide 

> In order to check authority using the ROLE service, 
> the RESTful API have to be called or the client SDK have to be used.

## AppKey & SecretKey

AppKey and SecretKey are required to use RESTful API and Client SDK.
You can check the key information issued at the top left of [CONSOLE].

![[Figure 1] Check AppKey and SecretKey](http://static.toastoven.net/prod_role/role_60.png)
<center>[Figure 1] Check AppKey and SecretKey</center>

## Client SDK

### What is Client SDK?

It is a ROLE-only client SDK for easy calling of RESTful API.
Since it has its own cache feature, it is possible to use the ROLE service more efficiently. 
Currently, we only support JAVA language.

### Usage Environment
`JDK 11` or later version environments

### Using the JAVA Client SDK with Maven

In order to use the JAVA client SDK, it is necessary to set the mean repository and dependency in pom.xml.

**[Maven Repository]** 
It is stored in the Maven Central Repository, so no additional settings are required.
If you use another storage or do not reference Maven Central environment, set it as follows.

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

### Using the JAVA Client SDK

To use the JAVA Client SDK, you first have to create an instance of the RoleClient object using the RoleClientFactory object.
Once you have created a RoleClient object, you may call the method provided by the object and process various tasks.

**[RoleConfig]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| appKey         | String  |**Yes**| App key issued by the server                                                       |
| secretKey      | String  |**Yes**| Secret key issued by the server                                                     |
| domain         | String  |**No**| Domain Address<br/>It uses default values and do not need to be set separately                   |
| connectTimeout | Integer |**No**| You can set the connection timeout, and the unit of time is millisecond.<br/>The default value is 10 seconds, which is the default value for okHttp   |
| readTimeout    | Integer |**No**| You can set Read timeout, and the unit of time is millisecond.<br/>The default value is 10 seconds, which is the default value for okHttp |

```java
String appKey = "appKey";
String secretKey = "secretKey";
 
// Correct way to create RoleClient objects
// Domain does not need to be set up separately.
RoleClient client = RoleClientFactory.getClient(RoleConfig.builder() 
                                                            .appKey(appKey) 
                                                            .secretKey(secretKey) 
                                                            .connectTimeout(30_000) 
                                                            .readTimeout(60_000) 
                                                            .build()); 
 
// You should not call the generator directly as below.
RoleClient client = new RoleClient(RoleConfig.builder() 
                                                .appKey(appKey) 
                                                .secretKey(secretKey) 
                                                .connectTimeout(30_000) 
                                                .readTimeout(60_000) 
                                                .build());
```

> Be careful not to call the RoleClient creator directly.

### SDK User Guide 
#### Common
> What is used as a common SDK feature

1. Request/response model for paging

**[Pageable]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| page         | Integer| **No** | Page number         |
| itemsPerPage | Integer | **No** | Number of items to be viewed per list |
| sort              | List&lt;String>      | **No** |    Data Sorting Criteria     |

**[Page]**

| Key          | Type | Required |   Description   |
|--------------|----------------|----|----------|
| totalItems         | Integer    | **Yes** | Total number    |
| items | List<T> | **Yes** | List viewed     |

#### 1. User
> Check user information Register, view, modify and delete and user role change history

1. Model

**[User]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|userId|     String |**Yes**| User ID|
|description|    String  |**No**| Description|
|regYmdt|  OffsetDateTime |**No**| User creatime time. Set on return |
|roleCounts|  List&lt;UserRoleCount> |**No**| Number of roles assgined to users     |
|roleRelations|  List<UserRoleRelation> |**No**| An associated role|

**[UserRoleCount]**

| Key       | Type     | Required | Description   |
|-----------|----------|----------|---------------|
| scopeId   | String   | **No**   | Scope ID         |
| roleCount | Integer  | **No**   | Number of roles for each scope ID  |

**[UserRoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String            |**Yes**|     Applicable target ID              |
|roleId|     String            |**Yes**|     Role ID                 |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|   Role enabled or not: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**|  Role Condition attributes              |
|regYmdt | Date| **Yes** | Created date  |

**[Condition]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId| String |**Yes**|   Condition Attribute ID          |
|attributeOperatorType | Required |**Yes**|   Condition Attribute Operator Type  |
|attributeValues| List&lt;String> |**No**|  Condition Attribute value           |

2. Create User

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

3. View user 

**[GetUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**Yes**|    User ID                                             |
| searchRoleOptionCode | SearchRoleOptionCode    |**No**|   Whether or not to search with associated permission: DIRECT_ROLE, INDIRRECT_ROLE |
| roleIds              | List&lt;String> |**No**|   List of Role IDs      |
| scopeIds             | List&lt;String> |**No**|   List of Applicable target IDs |

```java
GetUserRequest request = GetUserRequest.builder()
                                       .userId("")
                                       .build();

User user = client.getUser(request);
```

4. View User List

⚠️ See `Common` for models used when in response

**[GetUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds              | List&lt;String>      |**No**|   User ID list (Fully matched) |
| userIdPreLike        | String               |**No**|   User ID (Front matched) |
| scopeIds             | List&lt;String>      |**No**|   Range ID list (Fully matched) |
| scopeIdPreLike       | String               |**No**|   Range ID (Front matched) |
| roleIds              | List&lt;String>      |**No**|   Role ID list (fully matched) |
| roleIdPreLike        | String               |**No**|   Role ID (Front matched) |
| descriptionLike      | String               |**No**|   User Description (Partially matched) |
| searchRoleOptionCode | SearchRoleOptionCode |**No**|   Accessible Role List Search method |
| needRoleRelations    | Boolean              |**No**|   Whether or not to include role associations in response or not (default: true) |
| needRoleTags         | Boolean              |**No**|   When in response and include role associations, whether to include role tags or not (default: false)  |
| needRoleCount         | Boolean              |**No**| Whether or not to include the number of roles users have in responses (default: false)     |

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

5. Modify user

**[PutUserRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| user                 | User      |**Yes**|   ⚠️ Refer to `User` for the used model when requsted |
| createUserIfNotExist | Boolean    |**No**| Whether to create when the user does not exist when requested |


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

6. Delete user

```java
String userId = "";

client.deleteUser(userId);
```

7. Delete users

**[DeleteUsersRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds             | Set&lt;String>      |**Yes**| User IDs |

```java
DeleteUsersRequest request = DeleteUsersRequest.builder()
                                               .userIds(Set.of(""))
                                               .build();

client.deleteUsers(request);
```

8. Get a list of user role change history

**[GetUserRoleHistoriesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String |**No**|    User ID      |
| roleId              | String |**No**|     Role ID       |
| scopeId             | String   |**No**| Applicable target ID
| fromDateTime | OffsetDateTime   |**No**|  Start date and time of change     |
| toDateTime | OffsetDateTime   |**No**|    End date and time of change     |

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
| userHistorySeq               | long           |**Yes**|   Order           |
| userId               | String         |**Yes**|   User ID      |
| roleId              | String         |**No**|    Role ID       |
| scopeId             | String          |**No**| Applicable target ID
| roleApplyPolicyCode | RoleApplyPolicyCode |**No**|   Role enabled or not: ALLOW, DENY     |
| conditions | List&lt;ConditionBundle> |**No**|   Role Condition attributes     |
| command | UserRoleHistoryCommandCode |**Yes**|    Order     |
| executionTime | OffsetDateTime |**Yes**|  Modification date     |
| operatorUuid | String |**Yes**|   Operator UUID     |

9. Modify users based on scope

**[PutUserScopeRequest]**

| Key                  |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| userId               | String         |**Yes**|   User ID      |
| scopeId             | String          |**Yes**| Applicable ID
| description|    String  |**No**| 설명|
| createUserIfNotExist | Boolean    |**No**|  Whether to create when the user does not exist when requested |
| roleRelations|  List&lt;UserRoleRelation> |**No**| Related role|

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

#### 2. Operation
> Operation information Register, view, modify, and delete 

1. Model

**[Operation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationId|    String                    |**Yes**| Operation ID|
|description|    String                    |**No**| Description|

2. Create Operation

```java
Operation operation = Operation.builder()
                               .operationId("")
                               .description("")
                               .build();

client.createOperation(operation);
```

3. View Operation

⚠️ See `Model` for models used when in response

```java
String operationId = "";

Operation operation = client.getOperation(operationId);
```

4. View Operation list

⚠️ See `Common` for models used when in response

**[GetOperationsRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|operationIds|   List&lt;String> |**No**|   Operating ID list |
|operationIdPreLike|     String          |**No**|   Operation ID (Front matched) |
|descriptionLike|    String          |**No**|   Description (Partially matched)           |

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

5. Modify Operation

⚠️ See `Operation` for models used when request
```java
Operation operation = Operation.builder()
                .operationId("")
                .description("")
                .build();

client.updateOperation(operation);
```

6. Delete Operation

```java
String operationId = "";

client.deleteOperation(userId);
```

7. Delete operations

**[DeleteOperationsRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| operationIds             | Set&lt;String>      |**Yes**|  Operation IDs |

```java
DeleteOperationsRequest request = DeleteOperationsRequest.builder()
                                                          .operationIds(Set.of(""))
                                                          .build();

client.deleteOperations(request);
```

#### 3. Attribute
> Attribute register, view, modify, and delete

1. Model

**[Attribute]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String          |**Yes**|   Condition Attribute ID                                 |
|attributeName|  String          |**No**|   Condition attribute name                                 |
|description|    String          |**No**|   Description                                           |
|attributeDataType | AttributeDataType |**Yes**|   Condition attribute data type   |
|attributeCreationType | AttributeCreationType |**No**|   Condition attribute creation type   |
|attributeTagIds|    List&lt;String>    |**No**|    Condition attribute tag list                              |
|attributeRoleRelationIds|   List&lt;String> |**No**|   List of associated roles                                     |

2. Create attribute 

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

3. View attribute

```java
String attributeId = "";

Attribute attribute = client.getAttribute(attributeId);
```

4. View Attribute list 

⚠️ See `Common` for models used when in response

**[GetAttributesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeIds|   List&lt;String> |**No**|   Condition attribute ID list |
|attributeIdPreLike|     String          |**No**|   Condition attribute ID (Front matched) |
|roleIds|    List&lt;String> |**No**| Converse ID List       |
|roleIdPreLike|  String          |**No**|   Role ID (Front matched)           |
|attributeTagIds|    List&lt;String> |**No**|   Condition attribute ID list |
|descriptionLike|    String          |**No**|   Description (Partially matched)           |
|attributeDataType | AttributeDataType |**No**|   Condition attribute data type   |

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
| attribute                     |   Attribute                             |**Yes**| Condition attribute model          |
| attributeTagById              |   Map&lt;String, AttributeTag>          |**No**| Condition attribute tag information       |
| attributeRoleRelationByRoleId |   Map&lt;String, AttributeRoleRelation> |**No**| Roles associated with condition attribute |
| attributeInUse                |   Boolean                               |**Yes**| Condition attribute data type    |

5. Modify attribute

   ⚠️ See `Attribute` for models used when request

```java
Attribute attribute = Attribute.build()
                              .attributeId("")
                              .attributeName("")
                              .description("")
                              .build();

client.updateAttribute(attribute);
```

6. Delete attribute

**[DeleteAttributeRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeId|    String         |**Yes**|     Condition Attribute ID |
|forceDelete|    boolean        |**No**|     Whether or not forced to delete (default value: false) |

```java
DeleteAttributeRequest request = DeleteAttributeRequest.build()
                                                      .attributeId("")
                                                      .forceDelete(false)
                                                      .build();

client.deleteAttribute(request);
```

7. Delete attributes

**[DeleteAttributesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|attributeIds| Set&lt;String>      |**Yes**|  Condition attribute IDs |
|forceDelete|    boolean        |**No**|  Whether to force delete (Default: false) |

```java
DeleteAttributesRequest request = DeleteAttributesRequest.builder()
                                                          .operationIds(Set.of(""))
                                                          .build();

client.deleteAttributes(request);
```

#### 3. Scope
> Scope register, view, modify and delete

1. Model

**[Scope]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeId|    String          |**Yes**| Scope ID|
|description|    String      |**No**| Description|

2. Create Scope

```java
Scope scope = Scope.builder()
                   .scopeId("")
                   .description("")
                   .build();

client.createScope(scope);
```
3. View Scope

⚠️ See `Model` for models used when in response
```java
String scopeId = "";

Scope scope = client.getScope(scopeId);
```

4. View Scope list

⚠️ See `Common` for models used when in response

**[GetScopesRequest]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeIds|   List&lt;String> |**No**|   Scope ID list |
|scopeIdPreLike|     String          |**No**|   Range ID (Front matched) |
|descriptionLike|    String          |**No**|   Description (Partially matched)           |

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

5. Modify Scope

⚠️ See `Scope` for models used when request

```java
Scope scope = Scope.builder()
                  .scopeId("")
                  .description("")
                  .build();

client.updateScope(scope);
```

6. Delete Scope

```java
String scopeId = "";

client.deleteScope(userId);
```

7. Delete scopes

**[DeleteScopesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|scopeIds| Set&lt;String>      |**Yes**|   Scope IDs |

```java
DeleteScopesRequest request = DeleteScopesRequest.builder()
                                                 .scopeIds(Set.of(""))
                                                 .build();

client.deleteScopes(request);
```

#### 5. Role
> Role information register, view, modify, and delete, and View list of configurable attributes of registered Role, Available to change to DENY (not used) or not

1. Model

**[Role]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleMetaData|   RoleMetaData          |**Yes**|     Role          |
|roleRelations|  List<RoleRelation>    |**No**|  Association Role ID List |
|roleTags|   List<RoleTag> |**No**|  List of role tags        |

**[RoleMetaData]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleId|     String          |**Yes**|   Role ID          |
|roleName|   String          |**No**|   Role name          |
|roleGroup|  String |**No**|    Role Group |
|description|    String   |**No**|  Description              |
|exposureOrder|  Integer   |**No**|     Exposure Order       |

**[RoleRelation]**

|Key|    Type | Required |   Description   |
|--------------|----------------|----|----------|
|relatedRoleId|  String          |**Yes**|   Role Association ID          |
|roleApplyPolicyCode|    RoleApplyPolicyCode |**No**|   Role enabled or not: ALLOW, DENY |
|conditions|     List&lt;Condition>   |**No**|  Role Condition attributes              |

**[Condition]**

|Key|    Type | Required |   Description   | 
|--------------|----------------|----|----------| 
|attributeId| String |**Yes**|   Condition attribute ID          | 
|attributeOperatorType | Required |**Yes**|   Condition attribute operator type   | 
|attributeValues| List&lt;String> |**No**|  Condition attribute value           |

2. Create a Role

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

3. View role

⚠️ See `Model` for models used when in response

```java
String roleId = "";

Role role = client.getRole(roleId);
```

4. View Role list

**[GetRoleRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleIds           |    List&lt;String>  |**No**|  Role ID list (fully matched)                |
| roleIdPreLike     |    String           |**No**|  Range ID (Front matched)                  |
| relatedRoleIds    |    List&lt;String>  |**No**|  Association Role ID List (Fully matched)           |
| descriptionLike   |    String           |**No**|  Description (Partially matched)                        |
| roleNameLike      |    String           |**No**|  Role name (Partially matched)                   |
| roleGroupLike     |    String           |**No**|  Role group (Partially matched)                   |
| roleGroupLike     |    String           |**No**|  Role group(Partially matched)                   |
| roleTagIdExpr     |    String           |**No**|  Role tag condition (divider ';':OR, ',':AND) |
| roleTagIds        |    List&lt;String>  |**No**|  Role tag ID List (Fully matched)            |
| attributeIds      |    List&lt;String>  |**No**|  Role tag ID List (Fully matched)           |
| attributeTagIds   |    List&lt;String>  |**No**|  Condition attribute tag ID List (Fully matched)       |
| needAttributes    |    Boolean          |**No**|  Whether or not to include condition attribute information when in response           |
| needRoleTags      |    Boolean          |**No**|  Whether or not to include role tag ID list when in response         |
| needRoleRelations |    Boolean          |**No**|  Whether or not to include association role ID list when in response        |
| searchRoleOptionCode |    SearchRoleOptionCode          |**No**| Whether to include subroles when searching for roles  |

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

5. Update a Role

⚠️ See `Role` for models used when request

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

6. Delete a Role

```java
String roleId = "";

client.deleteRole(roleId);
```

7. Delete roles

**[DeleteRolesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|roleIds| Set&lt;String>      |**Yes**| Role IDs |

```java
DeleteRolesRequest request = DeleteRolesRequest.builder()
                                               .roleIds(Set.of(""))
                                               .build();

client.deleteRoles(request);
```

8. View a list of all attributes that can be set in the Role

**[GetRoleAttributesRequest]**

| Key               |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId            |    String          |**Yes**|   Role ID                     |
| attributeIds      |    List&lt;String> |**No**|   Role tag ID List (Fully matched)     |
| attributeTagIds   |    List&lt;String> |**No**|   Condition attribute tag ID List (Fully matched) |
| attributeNameLike |    Boolean         |**No**|   Condition attribute name when in response (Partially matched)   |

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
⚠️ See `3. Attribute ` Model for models used when in response

8. Used role or not Available to change to DENY (not used) or not

```java
String roleId = "";

boolean result = client.isDeniable(roleId);
```

#### 6. Role-related relations
> Register, modify, and delete role-related relations

1. Register role-related relations

**[CreateRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|  Role ID           |
| roleRelations   |    List&lt;RoleRelation>  |**No**|   ⚠️ Refer to RoleRelation Model for `5. ROle`   |

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

2. Modify role-related relations

**[UpdateRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|   Role ID                       |
| roleRelations   |    List&lt;RoleRelation>  |**No**|   ⚠️ Refer to RoleRelation Model for `5.Role` |

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

3. Delete role-related relations

**[DeleteRoleRelationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| roleId    |    String  |**Yes**|   Role ID                       |
| relatedRoleIds   |    List&lt;String>  |**No**|   Role-relation IDs |

```java
DeleteRoleRelationRequest role = DeleteRoleRelationRequest.builder()
                                                          .roleId("")
                                                          .relatedRoleIds(List.Of(""))
                                                          .build();

client.deleteRoleRelations(role);
```

#### 4. Resource
> Resource register, view, modify and delete

1. Model

**[Resource]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId    |    String  |**No**|   Resource ID                       |
| description   |    String  |**No**|   Description                                |
| name          |    String  |**No**|   Resource Name                       |
| path          |    String  |**Yes**|   Resource path                     |
| uiPath        |    String  |**Yes**|   Resource UI path                  |
| priority      |    Integer |**Yes**|   Priority                              |
| metadata      |    String  |**No**|   Metadata                             |
| newResourceId |    String  |**No**|   Use only when you want to update existing created resource ID |

2. Create Resource

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

3. View Resource

⚠️ See `Model` for models used when in response

```java
String resourceId = "";

Resource resource = client.getResource(resourceId);
```

4. View Resource list

⚠️ See `Common` for models used when in response

**[GetResourcesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceIdPreLike    |     String               |**No**|  Resource ID (Front matched)      |
| resourcePath         |     String               |**No**|  Resource path (Fully matched)    |
| resourcePathLike     |     String               |**No**|  Resource path (Front matched)         |
| resourceUiPath       |     String               |**No**|  Resource UI path (Front matched)      |
| resourceIds          |     List&lt;String>      |**No**|  Resource ID list                |
| resourcePaths        |     List&lt;String>      |**No**|  Resource path list (Fully matched)      |
| resourceUiPaths      |     List&lt;String>      |**No**|  Resource UI path list (Fully matched)   |
| userIds              |     List&lt;String>      |**No**|  List of user IDs with access to resources    |
| scopeIds             |     List&lt;String>      |**No**|  List of scope IDs with access to resources     |
| roleIds              |     List&lt;String>      |**No**|  List of role IDs assigned to resources        |
| operationIds         |     List&lt;String>      |**No**|  List of operation IDs assigned to resources |
| searchRoleOptionCode |     SearchRoleOptionCode |**No**|  Include sub roles when searching for roles or not      |

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

5. Modify resource

⚠️ See `Resource` for models used when request

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

6. Delete Resource

```java
String resourceId = "";

client.deleteResource(resourceId);
```

7. Delete resources

**[DeleteResourcesRequest]**

| Key                  |     Type | Required |   Description   |
|--------------|----------------|----|----------|
|resourceIds| Set&lt;String>      |**Yes**|   Resource IDs |

```java
DeleteResourcesRequest request = DeleteResourcesRequest.builder()
                                                       .roleIds(Set.of(""))
                                                       .build();

client.deleteResources(request);
```

#### 8. Resource hierarchy
> View hierarchy of resources. 
Hierarchy structure is formed based on uiPath (resourceUiPath), and is cached for user-defined cache time.

1. View Resource hierarchy

**[GetResourceHierarchyRequest]**

| Key             | Type | Required |   Description   |
|--------------|----------------|----|----------|
| userIds         | List&lt;String> |**No**| User ID list   |
| roleIds         | List&lt;String> |**No**| List of Role IDs    |
| operationIds    | List&lt;String> |**No**| Operating ID list |
| scopeIds        | List&lt;String> |**No**| Scope ID list    |
| resourceIds     | List&lt;String> |**No**| Resource ID list   |
| resourcePath    | String          |**No**| Resource path      |
| resourceUiPath  | String          |**No**| Resource UI path   |

```java
GetResourceHierarchyRequest request = GetResourceHierarchyRequest.builder()
                                                                .resourceIds(List.of(""))
                                                                .build());

List<ResourceHierarchy> responses = client.getResourceHierarchy(request);
```

**[ResourceHierarchy]**

| Key         | Type | Required |   Description   |
|--------------|----------------|----|----------|
| resourceId  | String                     |**Yes**| Resource ID                                  |
| description | String                     |**No**| Resource Description                                  |
| name        | String                     |**Yes**| Resource Name                                  |
| path        | String                     |**Yes**| Resource path                                  |
| uiPath      | String                     |**Yes**| Resource UI path<br/>Hierarchy is built based on this path.  |
| priority    | Integer                    |**Yes**| Priority                                    |
| Resources   | List<ResourceHierarchy> |**No**| Sub-resources                                 |

#### 9. User Authorization
> Verify that the user has a specific role or has access rights to the resource. 
In the case of resources, they are cached for a user-defined cache time.

1. Check authorization results for specific resource

* You have to have either a resource ID or a resource path when requesting it.
* If you put both the resource ID and the resource path value, the resource ID is checked first, therefore if you want to check with the resource path, you should not set the resource ID.

**[GetResourceAuthorizationRequest]**

| Key           |    Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId | String                          |**No**| User-defined ID value<br/>Use if you need to be clear about which authentication conditions are in response |
| operationId   | String                          |**Yes**| Operation ID                                              |
| resourceId    | String                          |**No**| Resource ID (required if resource path is not present)                             |
| resourcePath  | String                          |**No**| Resource path (required if resource ID is not present)                       |
| scopeId       | String                          |**No**| Scope ID                                                  |
| attributes    | List<AuthorizationAttribute> |**No**| Condition attribute list                                                     |

**[AuthorizationAttribute]**

| Key            |   Type | Required |   Description   |
|--------------|----------------|----|----------|
| attributeId    | String                           |**Yes**| Condition Attribute ID       |
| attributeValue | String                           |**Yes**| Condition Attribute value        |

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

2. Check authorization results for multiple resources
* Responses are returned in the requested sequence.

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
| authRequestId | String                          |**No**| User-defined ID value<br/>The value sent upon request is returned as is       |
| operationId   | String                          |**Yes**| Operation ID                                 |
| resourceId    | String                          |**Yes**| Resource ID                                       |
| resourcePath  | String                          |**No**| Resource path                                       |
| scopeId       | String                          |**Yes**| Scope ID                                        |
| permission    | Boolean                         |**Yes**| Authorization result<br/><br/>true: permission present<br/>false: permission not present |
| attributes    | List<AuthorizationAttribute> |**No**| Condition attribute list                                     |

3. Verify authorization results for a specific role

**[GetRoleAuthorizationRequest]**

| Key            | Type | Required |   Description   |
|--------------|----------------|----|----------|
| authRequestId  | String                          |**No**| User-defined ID value<br/>Use if you need to be clear about which authentication conditions are in response |
| attributes     | List<AuthorizationAttribute> |**No**| Condition attribute list                                                     |
| roleId         | String                          |**Yes**| Role ID                                                     |
| scopeId        | String                          |**No**| Scope ID                                                  |

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

4. Verify authorization results for multiple roles

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
| authRequestId  | String                          |**No**| User-defined ID value<br/>The value sent upon request is returned as is        |
| roleId         | String                          |**Yes**| Role ID                                         |
| scopeId        | String                          |**Yes**| Scope ID
| permission     | Boolean                         |**Yes**| Authorization result<br/><br/>true: permission present<br/>false: permission not present |
| attributes     | List<AuthorizationAttribute> |**No**| Condition attribute list                                      |

### Client SDK Cache

Client SDK uses the client-level cache for each of the following three cases.

- Check permissions using Resource ID
- Check permissions using Resource Path
- View Resource Hierarchy 

It is managed as an LRU, and default value for Cache is 300 seconds time to live (TTL) and 1,000,000 sizes.
If you want to modify this value, you can access NHN Cloud Console and change it. 
Any changes made in NHN Cloud Console will be reflected immediately and all existing caches will be deleted upon the changes being made.

![[Figure 2] Client SDK Cache Settings](http://static.toastoven.net/prod_role/role_62.png)
<center>[Figure 2] Client SDK Cache Settings</center>

### Support Transaction

If you want to add, change, or delete the data in the ROLE atomically, you can get and use RoleSession object by calling BeginTransaction() of RoleClient object.

For example, if you register multiple Roles at the same time as below, some may be registered and some may not be registered in the event of an error in the middle.

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
 
	// If Exception occurs here 
	// U1 will be created but M1 will not be created. 
	client.createRole(role); 
} catch (Exception e) { 
    // When error occurred, you have to implement own Rollback logic. 
    client.userDelete("U1"); 
}
```

By using RoleSession object, you can eliminate partial failures in the above situations.

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
 
   // Even if Exception occurs here, partial failure will not happen. 
   session.createRole(role); 
   // If no error occurred, the change items will be reflected on the server. 
   session.commit(); 
} catch (Exception e) { 
   // When an error occurs, the changes stored in the session are emptied through the rollback function   
   session.rollback(); 
}
```

When using RoleSession object, be careful not to read the changed data before commit() because no additions/modifications/changes are reflected in the server until the commit() method is called.

You can commit() or rollback() the RoleSession object and then reuse it.

> RoleSession is equally available for registration, modification, and deletion of services defined in `SDK User Guide ` except for queries.
