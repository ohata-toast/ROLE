## Application Service > ROLE > API v3 가이드

> ROLE 서비스를 이용해 권한을 체크하기 위해서는
> RESTful API를 호출하거나, 클라이언트 SDK를 이용하여야 합니다.

## 앱키 & 비밀 키

RESTful API와 클라이언트 SDK를 사용하려면 앱키와 비밀 키가 필요합니다.
[CONSOLE] 우측 상단의 **URL & Appkey** 버튼을 클릭하여 발급 키 정보를 확인 할 수 있습니다.

![[그림 1] 앱키 & 비밀 키 확인](http://static.toastoven.net/prod_role/role_60.png)
<center>[그림 1] 앱키 & 비밀 키 확인</center>

## RESTful API 가이드

### Common Response Body

모든 API 요청에 대해 HTTP 응답 코드는 200으로 응답합니다.
자세한 응답 결과는 Response Body의 Header 항목을 참고합니다.

```json
{
  "cache": {
    "cacheFlushTime": "",
    "size": 0,
    "sizeTree": 1,
    "ttl": 5,
    "sizeByPath": 6
  },
  "header": {
    "isSuccessful": true,
    "resultCode": 5,
    "resultMessage": "resultMessage"
  }
}
```

| Key                  |  Type    | Description                            |
|----------------------|----------|----------------------------------------|
| header               |  Object  | 응답 헤더                                  |
| header.isSuccessful  |  boolean | 성공 여부                                  |
| header.resultCode    |  int     | 응답 코드. 성공 시 0, 실패 시 오류 코드 반환           |
| header.resultMessage |  String  | 응답 메시지. 성공 시 "SUCCESS", 실패 시 오류 메시지 반환 |
| cache                | Object   | 캐시                                     |
| cache.cacheFlushTime | String   | 캐시 삭제 시간                               | 
| cache.size | int      | 리소스 ID 기반 인증 캐시 크기                     |
| cache.sizeByPath | int      | 리소스 Path 기반 인증 캐시 크기                   |
| cache.sizeTree | int      | 리소스 Hierarchy 조회 캐시 크기                 |
| cache.ttl | int      | 캐시 데이터 유지 시간(초 단위)                     |

## 사용자


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users**](#createUsers) | 사용자 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#deleteUser) | 사용자 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users**](#deleteUsers) | 사용자 다건 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/id**](#getAllUsers) | 모든 사용자 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#getUser) | 사용자 정보 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/histories**](#getUserRoleHistories) | 사용자에게 할당된 역할의 변경 내역 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/search**](#getUsers) | 사용자 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](#updateUser) | 사용자 수정 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}**](#updateUserScope) | 사용자 범위 한정 수정 |


<a name="createUsers"></a>
### **사용자 생성**
> POST "/role/v3.0/appkeys/{appKey}/users"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateUserRequest** | **CreateUserRequest**| **Yes** |  | |





##### CreateUserRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **users** | **List&lt;CreateUserRequest.UserProtocol>**| **Yes** | 사용자 목록  |

##### CreateUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | 사용자 연관 역할  |
|   **userId** | **String**| **Yes** | 사용자 ID  |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID |
|   **scopeId** | **String**| **Yes** |  범위 ID    |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |



























#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteUser"></a>
### **사용자 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/users/{userId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteUsers"></a>
### **사용자 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/users"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**userIds** |  **List&lt;String>**| **Yes** | 사용자 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getAllUsers"></a>
### **모든 사용자 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/users/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 사용자 설명(부분 일치)  |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 여부  |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 시 역할 태그 포함 여부  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)  |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | 사용자 ID(전방 일치)  |
|   **userIds** | **List&lt;String>**| **No** | 사용자 ID 목록(완전 일치)  |



















#### Response Body

```json
{
  "totalItems" : 0,
  "userIds" : [ "userIds", "userIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAllUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **userIds** | **List&lt;String>**| **Yes** | 사용자 목록  |











<a name="getUser"></a>
### **사용자 정보 조회**
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
|  Query |**searchRoleOptionCode** | **String**| **No** | 접근 가능한 역할 목록 검색 방식 | [optional] [default to null] [enum: DIRECT_ROLE, INDIRECT_ROLE] |
|  Query |**roleIds** |  **List&lt;String>**| **No** | 연관 관계 역할 ID |
|  Query |**scopeIds** |  **List&lt;String>**| **No** | 연관 관계 범위 ID |










#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "user" : {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId"
  }
}
```



##### GetUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **user** | **UserBundleProtocol**| **Yes** | 사용자         |


##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 설명  |
|   **regYmdt** | **Date**| **No** | 사용자 생성 일시  |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | 사용자에 할당된 역할 목록  |
|   **userId** | **String**| **Yes** | 사용자 ID  |



##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **No** | 등록일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |























<a name="getUserRoleHistories"></a>
### **사용자에게 할당된 역할의 변경 내역 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}/histories"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
|  Query |**roleId** | **String**| **No** | 역할 ID |
|  Query |**scopeId** | **String**| **No** | 범위 ID |
|  Query |**fromDateTime** | **Date**| **No** | 변경 시작일시 |
|  Query |**toDateTime** | **Date**| **No** | 변경 종료일시 |
|  Query |**historyType** |  **List&lt;String>**| **No** | 변경 유형 | [optional] [default to null] [enum: USER_ADD, USER_REMOVE, ADD, REMOVE] |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `seq,DESC`)|















#### Response Body

```json
{
  "totalItems" : 0,
  "userHistory" : [ {
    "executionTime" : "2000-01-23T04:56:07.000+00:00",
    "scopeId" : "scopeId",
    "userHistorySeq" : 6,
    "roleId" : "roleId",
    "operatorUuid" : "operatorUuid",
    "conditions" : [ {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    }, {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    } ],
    "userId" : "userId"
  }, {
    "executionTime" : "2000-01-23T04:56:07.000+00:00",
    "scopeId" : "scopeId",
    "userHistorySeq" : 6,
    "roleId" : "roleId",
    "operatorUuid" : "operatorUuid",
    "conditions" : [ {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    }, {
      "attributeId" : "instance.name",
      "attributeValues" : [ "attributeValues", "attributeValues" ],
      "attribute" : {
        "attributeId" : "attributeId",
        "description" : "description",
        "attributeName" : "attributeName"
      }
    } ],
    "userId" : "userId"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetUserHistory.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **userHistory** | **List&lt;UserHistoryProtocol>**| **Yes** | 사용자 변경 내역 목록  |



##### UserHistoryProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **command** | **String**| **Yes** |   USER_ADD, USER_REMOVE, ADD, REMOVE |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **executionTime** | **Date**| **Yes** | 변경 일시  |
|   **operatorUuid** | **String**| **No** | 작업자 UUID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |
|   **roleId** | **String**| **No** | 역할 ID  |
|   **scopeId** | **String**| **No** | 범위 ID  |
|   **userHistorySeq** | **Long**| **Yes** | 사용자 변경 이력 일렬번호  |
|   **userId** | **String**| **Yes** | 사용자 ID  |


##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |




<a name="getUsers"></a>
### **사용자 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/users/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.userId,ASC`)|
| Request Body | **SearchUser.Request** | **SearchUser.Request**| **Yes** |  | |





##### SearchUser.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 사용자 설명(부분 일치)  |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 여부  |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 연관 관계 포함 시 역할 태그 포함 여부  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)  |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| **No** | 사용자 ID(전방 일치)  |
|   **userIds** | **List&lt;String>**| **No** | 사용자 ID 목록(완전 일치)  |




















#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "users" : [ {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId"
  }, {
    "roleRelations" : [ {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "scopeId" : "scopeId",
      "exposureOrder" : 6,
      "roleTags" : [ {
        "roleTagId" : "roleTagId"
      }, {
        "roleTagId" : "roleTagId"
      } ],
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "userId" : "userId"
  } ]
}
```



##### SearchUser.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |
|   **users** | **List&lt;UserBundleProtocol>**| **Yes** | 사용자 목록  |



##### UserBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 설명  |
|   **regYmdt** | **Date**| **No** | 사용자 생성 일시  |
|   **roleRelations** | **List&lt;UserBundleProtocol.UserRoleRelationBundleProtocol>**| **No** | 사용자에 할당된 역할 목록  |
|   **userId** | **String**| **Yes** | 사용자 ID  |



##### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **No** | 등록일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleTags** | **List&lt;UserBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |
























##### UserBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |























<a name="updateUser"></a>
### **사용자 수정**
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}"

#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키          | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID      | 
| Request Body | **PutUserRequest** | **PutUserRequest**| **Yes** | 사용자         |






##### PutUserRequest


| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserRequest.UserProtocol**| **Yes** | 사용자         |
| **createUserIfNotExist** | **Boolean** | **No** | 요청 시 존재하지 않는 사용자일 경우 생성 여부 |

##### PutUserRequest.UserProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserRoleRelationProtocol>**| **No** | 사용자 연관 역할  |


##### UserRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성    |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID       |
|   **scopeId** | **String**| **Yes** | 범위 ID       |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |


























#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



<a name="updateUserScope"></a>
### **사용자 범위 한정 수정**
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}/scopes/{scopeId}"

#### Parameters

| ParameterType | Name | Type | Required | Description |
|------------- |------------- | ------------- | ------------- |-------------|
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**userId** | **String**| **Yes** | 사용자 ID |
|  Path |**scopeId** | **String**| **Yes** | 범위 ID |
| Request Body | **putUserScopeRequest** | **PutUserScopeRequest**| **Yes** | 사용자 |

##### PutUserScopeRequest

| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **user** | **PutUserScopeRequest.UserProtocol**| **Yes** | 사용자 |
| **createUserIfNotExist** | **Boolean** | **No** | 요청 시 존재하지 않는 사용자일 경우 생성 여부 |

##### PutUserScopeRequest.UserProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 사용자 설명  |
|   **roleRelations** | **List&lt;UserScopeRoleRelationProtocol>**| **No** | 사용자 연관 역할  |


##### UserScopeRoleRelationProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- |-------------|
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성 |
|   **roleApplyPolicyCode** | **String**| **No** | ALLOW, DENY |
|   **roleId** | **String**| **Yes** | 역할 ID |

##### ConditionProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |

#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 사용자 인증


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources**](#checkResource) | 사용자가 리소스에 접근 권한이 있는지 검사 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles**](#checkRole) | 사용자가 역할에 대한 접근 권한이 있는지 검사 |


<a name="checkResource"></a>
### **사용자가 리소스에 접근 권한이 있는지 검사**
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources"

#### Parameters



| ParameterType | Name | Type | Required | Description | 
|------------- |------------- | ------------- | ------------- |-------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키   |
|  Path |**appKey** | **String**| **Yes** | 앱키          | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID      | 
| Request Body | **PostAuthorizationResource.Request** | **PostAuthorizationResource.Request**| **Yes** | 리소스 목록      | |






##### PostAuthorizationResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **resources** | **List&lt;PostAuthorizationResource.ResourceProtocol>**| **Yes** | 리소스 목록      |

##### PostAuthorizationResource.ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **No** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |
|   **scopeId** | **String**| **No** | 범위 ID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |























#### Response Body

```json
{
  "authorizations" : [ {
    "resourceId" : "resourceId",
    "scopeId" : "scopeId",
    "resourcePath" : "resourcePath",
    "authRequestId" : "authRequestId",
    "operationId" : "operationId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  }, {
    "resourceId" : "resourceId",
    "scopeId" : "scopeId",
    "resourcePath" : "resourcePath",
    "authRequestId" : "authRequestId",
    "operationId" : "operationId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostAuthorizationResource.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;PostAuthorizationResource.AuthorizationWithResourceProtocol>**| **No** | 권한 체크 결과 목록  |

##### PostAuthorizationResource.AuthorizationWithResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationResource.AttributeProtocol>**| **Yes** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **permission** | **Boolean**| **Yes** | 권한 여부  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### PostAuthorizationResource.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |

























<a name="checkRole"></a>
### **사용자가 역할에 대한 접근 권한이 있는지 검사**
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**userId** | **String**| **Yes** | 사용자 ID | 
| Request Body | **PostAuthorizationRole.Request** | **PostAuthorizationRole.Request**| **Yes** |  | |






##### PostAuthorizationRole.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List&lt;PostAuthorizationRole.AuthRoleProtocol>**| **Yes** | 인증 요청 목록  |

##### PostAuthorizationRole.AuthRoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **No** | 조건 속성  |
|   **authRequestId** | **String**| **No** | 인증 요청 식별키  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **scopeId** | **String**| **No** | 범위 ID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |





















#### Response Body

```json
{
  "authorizations" : [ {
    "scopeId" : "scopeId",
    "roleId" : "roleId",
    "authRequestId" : "authRequestId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  }, {
    "scopeId" : "scopeId",
    "roleId" : "roleId",
    "authRequestId" : "authRequestId",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    }, {
      "attributeId" : "attributeId",
      "attributeValue" : "attributeValue"
    } ],
    "permission" : true
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostAuthorizationRole.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;PostAuthorizationRole.AuthorizationProtocol>**| **No** | 권한 체크 결과 목록  |

##### PostAuthorizationRole.AuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;PostAuthorizationRole.AttributeProtocol>**| **Yes** | 조건 속성 ID  |
|   **authRequestId** | **String**| **No** | 요청 인증 식별키  |
|   **permission** | **Boolean**| **Yes** | 권한 여부  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |

##### PostAuthorizationRole.AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeValue** | **String**| **Yes** | 조건 속성 값  |






















## 역할


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles**](#createRole) | 역할 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#deleteRole) | 역할 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles**](#deleteRoles) | 역할 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable**](#getDeniable) | 역할 사용여부 DENY(미사용)로 변경가능 여부 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#getRole) | 역할 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/id**](#searchAllRoleIds) | 모든 역할 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search**](#searchAttributesByRoleId) | 역할에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/search**](#searchRoles) | 역할 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](#updateRole) | 역할 수정 |


<a name="createRole"></a>
### **역할 생성**
> POST "/role/v3.0/appkeys/{appKey}/roles"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateRoleRequest** | **CreateRoleRequest**| **Yes** |  | |





##### CreateRoleRequest


| Name | Type | Required | Description      | 
|------------ | ------------- | ------------- |------------------|
|   **role** | **RoleProtocol**| **No** | 역할 |
|   **roleRelations** | **List&lt;CreateRoleRequest.RoleRelationProtocol>**| **No** | 조건 속성과 연관된 역할 ID 목록 |
|   **roleTags** | **List&lt;CreateRoleRequest.RoleTagProtocol>**| **No** | 역할 태그 목록         |

##### RoleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |










##### CreateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |














##### CreateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | 역할 태그 ID  |













#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteRole"></a>
### **역할 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteRoles"></a>
### **역할 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**roleIds** |  **List&lt;String>**| **Yes** | 역할 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getDeniable"></a>
### **역할 사용여부 DENY(미사용)로 변경가능 여부**
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 









#### Response Body

```json
{
  "deniable" : true,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetDeniableResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **deniable** | **Boolean**| **No** | 역할 사용여부 DENY(미사용)로 변경가능 여부  |










<a name="getRole"></a>
### **역할 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 









#### Response Body

```json
{
  "role" : {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetRoleResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **role** | **RoleBundleProtocol**| **Yes** | 역할 |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | 앱키  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 조건 속성 목록  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 연관 관계 역할 목록  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |

















<a name="searchAllRoleIds"></a>
### **모든 역할 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.roleId,ASC`)|











#### Response Body

```json
{
  "totalItems" : 0,
  "roleIds" : [ "roleIds", "roleIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllRoleIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIds** | **List&lt;String>**| **Yes** | 역할 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |











<a name="searchAttributesByRoleId"></a>
### **역할에서 설정 가능한 모든 조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search"

#### Parameters



| ParameterType | Name | Type | Required | Description                                                | 
|------------- |------------- | ------------- | ------------- |------------------------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키                                                  |
|  Path |**appKey** | **String**| **Yes** | 앱키                                                         | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID                                                      | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1)                                      | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10)                                 |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서 (기본값 `attributeCreationTypeCode,ASC&quot;,&quot;id.attributeId,ASC`)|
| Request Body | **SearchRoleAttributes.Request** | **SearchRoleAttributes.Request**| **Yes** |                                                            | |






##### SearchRoleAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeNameLike** | **String**| **No** | 조건 속성 이름(부분 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |













#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchRoleAttributes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | 역할에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |



















<a name="searchRoles"></a>
### **역할 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/roles/search"

#### Parameters



| ParameterType | Name | Type | Required | Description                                   | 
|------------- |------------- | ------------- | ------------- |-----------------------------------------------| 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키                                     |
|  Path |**appKey** | **String**| **Yes** | 앱키                                            | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1)                         | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10)                    |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서 (기본값 `exposureOrder,ASC&quot;,&quot;id.roleId,ASC`)|
| Request Body | **GetRoles.Request** | **GetRoles.Request**| **Yes** |                                               | |





##### GetRoles.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 역할 설명(부분 일치)  |
|   **needAttributes** | **Boolean**| **No** | 응답 시 조건 속성 정보 포함 여부  |
|   **needRoleRelations** | **Boolean**| **No** | 응답 시 역할 연관 관계 ID 목록 포함 여부  |
|   **needRoleTags** | **Boolean**| **No** | 응답 시 역할 태그 ID 목록 포함 여부  |
|   **relatedRoleIds** | **List&lt;String>**| **No** | 연관 관계 역할 ID 목록(완전 일치)  |
|   **roleGroup** | **String**| **No** | 역할 그룹(완전 일치)  |
|   **roleGroupLike** | **String**| **No** | 역할 그룹(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |
|   **roleNameLike** | **String**| **No** | 역할 이름(부분 일치)  |
|   **roleTagIdExpr** | **String**| **No** | 역할 태그 조건(구분자 &#39;;&#39;:OR, &#39;,&#39;:AND)  |
|   **roleTagIds** | **List&lt;String>**| **No** | 역할 태그 ID 목록(완전 일치)  |























#### Response Body

```json
{
  "totalItems" : 6,
  "roles" : [ {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  }, {
    "regDateTime" : "2000-01-23T04:56:07.000+00:00",
    "roleRelations" : [ {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    }, {
      "regDateTime" : "2000-01-23T04:56:07.000+00:00",
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "conditions" : [ {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      }, {
        "attributeId" : "instance.name",
        "attributeValues" : [ "attributeValues", "attributeValues" ],
        "attribute" : {
          "attributeId" : "attributeId",
          "description" : "description",
          "attributeName" : "attributeName"
        }
      } ],
      "roleGroup" : "roleGroup"
    } ],
    "exposureOrder" : 0,
    "roleTags" : [ {
      "roleTagId" : "roleTagId"
    }, {
      "roleTagId" : "roleTagId"
    } ],
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "appKey" : "appKey",
    "attributes" : [ {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    }, {
      "attributeId" : "attributeId",
      "description" : "description",
      "attributeName" : "attributeName"
    } ],
    "roleGroup" : "roleGroup"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetRoles.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roles** | **List&lt;RoleBundleProtocol>**| **Yes** | 역할 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |


##### RoleBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **Yes** | 앱키  |
|   **attributes** | **List&lt;AttributeProtocol>**| **No** | 조건 속성 목록  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |
|   **roleRelations** | **List&lt;RoleBundleProtocol.RoleRelationProtocol>**| **No** | 연관 관계 역할 목록  |
|   **roleTags** | **List&lt;RoleBundleProtocol.RoleTagProtocol>**| **No** | 역할 태그 목록  |


##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |
















##### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionBundleProtocol>**| **No** | 역할 조건 속성  |
|   **description** | **String**| **No** | 역할 설명  |
|   **regDateTime** | **Date**| **Yes** | 역할 생성 일시  |
|   **roleApplyPolicyCode** | **String**| **Yes** |   ALLOW, DENY |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |

##### ConditionBundleProtocol


| Name | Type | Required | Description                                                                                                                                                                               | 
|------------ | ------------- | ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **attribute** | **AttributeProtocol**| **Yes** | 조건 속성                                                                                                                                                                                     |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID                                                                                                                                                                                  |
|   **attributeOperatorTypeCode** | **String**| **Yes** | ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값                                                                                                                                                                                   |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |



























##### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **No** | 역할 태그 ID  |


















<a name="updateRole"></a>
### **역할 수정**
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**roleId** | **String**| **Yes** | 역할 ID | 
| Request Body | **UpdateRoleRequest** | **UpdateRoleRequest**| **Yes** |  | |






##### UpdateRoleRequest


| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **role** | **RoleMetadataProtocol**| **No** | 역할                  |
|   **roleRelations** | **List&lt;UpdateRoleRequest.RoleRelationProtocol>**| **No** | 조건 속성과 연관된 역할 ID 목록 |
|   **roleTags** | **List&lt;UpdateRoleRequest.RoleTagProtocol>**| **No** | 역할 태그 목록            |

##### RoleMetadataProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleName** | **String**| **No** | 역할 이름  |









##### UpdateRoleRequest.RoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |














##### UpdateRoleRequest.RoleTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagId** | **String**| **Yes** | 역할 태그 ID  |













#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


## 역할 태그


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/tags/id**](#getAllRoleTagIds) | 모든 역할 태그 ID 목록 조회 |


<a name="getAllRoleTagIds"></a>
### **모든 역할 태그 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/tags/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**roleTagIdPreLike** | **String**| **No** | 역할 태그 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.roleTagId,ASC`)|











#### Response Body

```json
{
  "totalItems" : 0,
  "roleTagIds" : [ "roleTagIds", "roleTagIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllRoleTagIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleTagIds** | **List&lt;String>**| **No** | 역할 태그 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |




## 역할 연관 관계


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#createRoleRelations) | 역할 연관 관계 다건 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations}**](#deleteRoleRelations) | 역할 연관 관계 다건 삭제 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations**](#updateRoleRelations) | 역할 연관 관계 다건 수정 |

<a name="createRoleRelations"></a>
### **역할 연관 관계 다건 생성**
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **CreateRoleRelationRequest** | **CreateRoleRelationRequest**| **Yes** |  | |

##### CreateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | 역할 연관 관계 목록 |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

<a name="deleteRoleRelations"></a>
### **역할 연관 관계 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **DeleteRoleRelationRequest** | **DeleteRoleRelationRequest**| **Yes** |  | |

##### DeleteRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **relatedRoleIds** | **List&lt;String>**| **Yes** | 연관 관계 역할 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="updateRoleRelations"></a>
### **역할 연관 관계 다건 수정**
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}/relations"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
|  Path |**roleId** | **String**| **Yes** | 역할 ID |
| Request Body | **UpdateRoleRelationRequest** | **UpdateRoleRelationRequest**| **Yes** |  | |

##### UpdateRoleRelationRequest

| Name | Type | Required | Description      |
|------------ | ------------- | ------------- |------------------|
|   **roleRelations** | **List&lt;RoleRelationProtocol>**| **Yes** | 역할 연관 관계 목록 |


##### RoleRelationProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **No** | 역할 조건 속성  |
|   **relatedRoleId** | **String**| **Yes** | 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**| **No** |   ALLOW, DENY |

##### ConditionProtocol

| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



## 범위


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](#createScope) | 범위 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#deleteScope) | 범위 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes**](#deleteScopes) | 범위 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](#getAllScopeIds) | 모든 범위 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#getScope) | 범위 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](#postSearchScopes) | 범위 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](#updateScope) | 범위 수정 |


<a name="createScope"></a>
### **범위 생성**
> POST "/role/v3.0/appkeys/{appKey}/scopes"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateScope.Request** | **CreateScope.Request**| **Yes** |  | |





##### CreateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |










#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteScope"></a>
### **범위 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteScopes"></a>
### **범위 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/scopes"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**scopeIds** |  **List&lt;String>**| **Yes** | 범위 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getAllScopeIds"></a>
### **모든 범위 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.scopeId,ASC`)|











#### Response Body

```json
{
  "totalItems" : 0,
  "scopeIds" : [ "scopeIds", "scopeIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAllScopeIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |











<a name="getScope"></a>
### **범위 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 









#### Response Body

```json
{
  "scope" : {
    "scopeId" : "scopeId",
    "description" : "description"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetScope.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **scope** | **ScopeProtocol**| **No** | 범위          |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |














<a name="postSearchScopes"></a>
### **범위 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.scopeId,ASC`)|
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**| **Yes** |  | |





##### PostSearchScopes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 범위 설명(부분 일치)  |
|   **scopeIdPreLike** | **String**| **No** | 범위 ID(전방 일치)  |
|   **scopeIds** | **List&lt;String>**| **No** | 범위 ID 목록(완전 일치)  |













#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "scopes" : [ {
    "scopeId" : "scopeId",
    "description" : "description"
  }, {
    "scopeId" : "scopeId",
    "description" : "description"
  } ]
}
```



##### PostSearchScopes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **scopes** | **List&lt;ScopeProtocol>**| **No** | 범위 목록  |
|   **totalItems** | **Long**| **No** | 범위 총 개수  |


##### ScopeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 범위 설명  |
|   **scopeId** | **String**| **Yes** | 범위 ID  |















<a name="updateScope"></a>
### **범위 수정**
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**scopeId** | **String**| **Yes** | 범위 ID | 
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**| **Yes** |  | |






##### UpdateScope.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 설명  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 리소스


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources**](#createResource) | 리소스 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#deleteResource) | 리소스 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources**](#deleteResources) | 리소스 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#getResource) | 리소스 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/id**](#getResourceIds) | 리소스 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources**](#getResources) | 리소스 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/attributes/search**](#searchAttributesByResource) | 리소스에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/search**](#searchResources) | 리소스 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](#updateResource) | 리소스 수정 |


<a name="createResource"></a>
### **리소스 생성**
> POST "/role/v3.0/appkeys/{appKey}/resources"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateResource.Request** | **CreateResource.Request**| **Yes** |  | |





##### CreateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |















#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteResource"></a>
### **리소스 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteResources"></a>
### **리소스 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/resources"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**resourceIds** |  **List&lt;String>**| **Yes** | 리소스 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getResource"></a>
### **리소스 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 









#### Response Body

```json
{
  "resource" : {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  },
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetResource.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **resource** | **ResourceProtocol**| **No** | 리소스         |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |



















<a name="getResourceIds"></a>
### **리소스 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.resourceId,ASC`)|
| Request Body | **GetAllResourceIds.Request** | **GetAllResourceIds.Request**| **Yes** |  | |





##### GetAllResourceIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operationIds** | **List&lt;String>**| **No** | 리소스 ID(전방 일치)      |
|   **resourceIdPreLike** | **String**| **No** | 리소스에 접근 가능한 사용자 ID      |
|   **roleIds** | **List&lt;String>**| **No** | 리소스에 부여된 역할 ID      |
|   **userIds** | **List&lt;String>**| **No** | 리소스에 부여된 Operation ID      |














#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resourceIds" : [ "resourceIds", "resourceIds" ]
}
```



##### GetAllResourceIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resourceIds** | **List&lt;String>**| **Yes** | 리소스 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |











<a name="getResources"></a>
### **리소스 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**userId** | **String**| **No** | 리소스에 접근 가능한 사용자 ID |
|  Query |**roleId** | **String**| **No** | 리소스에 부여된 역할 ID |
|  Query |**operationId** | **String**| **No** | 리소스에 부여된 Operation ID |











#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ]
}
```



##### GetResources.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **List&lt;ResourceProtocol>**| **No** | 리소스 목록  |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |



















<a name="searchAttributesByResource"></a>
### **리소스에서 설정 가능한 모든 조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/attributes/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchResourceAttributes.Request** | **SearchResourceAttributes.Request**| **Yes** |  | |





##### SearchResourceAttributes.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **No** | 리소스 ID, ID와 Path 가 둘다 있을 경우 ID 기준으로만 제공  |
|   **resourcePath** | **String**| **No** | 리소스 Path  |













#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchResourceAttributes.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeProtocol>**| **Yes** | 리소스에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |

##### AttributeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **description** | **String**| **No** | 조건 속성 설명  |



















<a name="searchResources"></a>
### **리소스 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `uiPath,ASC`)|
| Request Body | **PostSearchResources.Request** | **PostSearchResources.Request**| **Yes** |  | |





##### PostSearchResources.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **No** | 리소스에 부여된 Operation ID 목록  |
|   **resourceIdPreLike** | **String**| **No** | 리소스 ID(전방 일치)  |
|   **resourceIds** | **List&lt;String>**| **No** | 리소스 ID 목록  |
|   **resourcePath** | **String**| **No** | 리소스 Path(완전 일치)  |
|   **resourcePathLike** | **String**| **No** | 리소스 Path(전방 일치)  |
|   **resourcePaths** | **List&lt;String>**| **No** | 리소스 Path 목록(완전 일치)  |
|   **resourceUiPath** | **String**| **No** | 리소스 UI Path(완전 일치)  |
|   **resourceUiPaths** | **List&lt;String>**| **No** | 리소스 UI Path 목록(완전 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 리소스에 부여된 역할 ID 목록  |
|   **scopeIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 범위 ID 목록  |
|   **searchRoleOptionCode** | **String**| **No** | 접근 가능한 역할 목록 검색 방식  DIRECT_ROLE, INDIRECT_ROLE |
|   **userIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 사용자 ID 목록  |






















#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ]
}
```



##### PostSearchResources.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **List&lt;ResourceProtocol>**| **Yes** | 리소스 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |




















<a name="updateResource"></a>
### **리소스 수정**
> PUT "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
| Request Body | **UpdateResource.Request** | **UpdateResource.Request**| **Yes** |  | |






##### UpdateResource.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **newResourceId** | **String**| **No** | 변경할 리소스 ID  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |















#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 리소스 계층 구조


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources**](#getSubResources) | ui path 상의 하위 리소스 페이지 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/hierarchy/search**](#searchAllResourceHierarchy) | 리소스 Hierarchy 조회 |


<a name="getSubResources"></a>
### **ui path 상의 하위 리소스 페이지 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/sub-resources"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
|  Query |**userId** | **String**| **No** | 사용자 ID |
|  Query |**roleId** | **String**| **No** | 역할 ID |
|  Query |**operationId** | **String**| **No** | 오퍼레이션 ID |
|  Query |**scopeId** | **String**| **No** | 범위 ID |
|  Query |**depth** | **Integer**| **No** | 리소스 UI Path에서 하위의 계층 깊이 |
|  Query |**limit** | **Integer**| **No** | 반환할 목록의 위치. default: INT_MAX |
|  Query |**offset** | **Integer**| **No** | 반환할 목록의 시작 위치. default: 0 |
















#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ],
  "totalItemCount" : 0
}
```



##### GetSubResources.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **List&lt;ResourceProtocol>**| **No** | 리소스 목록  |
|   **totalItemCount** | **Long**| **No** | 리소스 전체 개수  |


##### ResourceProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |




















<a name="searchAllResourceHierarchy"></a>
### **리소스 Hierarchy 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/hierarchy/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **SearchResourceHierarchy.Request** | **SearchResourceHierarchy.Request**| **Yes** |  | |





##### SearchResourceHierarchy.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
| **operationIds** | **List&lt;String>**| **No** | 리소스에 할당된 오퍼레이션 ID 목록 |
| **resourceIds** | **List&lt;String>**| **No** | 계층 구조의 Root Resource ID 목록 |
| **resourcePath** | **String**| **No** | 계층 구조의 Root Resource Path |
| **resourceUiPath** | **String**| **No** | 계층 구조의 Root Resource Ui Path |
| **roleIds** | **List&lt;String>**| **No** | 리소스에 할당된 역할 ID 목록 |
| **scopeIds** | **List&lt;String>**| **No** | 사용자에게 할당된 범위 ID 목록 |
| **userIds** | **List&lt;String>**| **No** | 리소스에 접근 가능한 사용자 ID 목록 |















#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "resources" : [ {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "resources" : [ null, null ],
    "priority" : -27519,
    "uiPath" : "uiPath"
  }, {
    "path" : "path",
    "metadata" : "metadata",
    "resourceId" : "resourceId",
    "name" : "name",
    "description" : "description",
    "resources" : [ null, null ],
    "priority" : -27519,
    "uiPath" : "uiPath"
  } ]
}
```



##### SearchResourceHierarchy.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 리소스 계층 구조 목록  |


##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |







##### SearchResourceHierarchy.ResourceHierarchyProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 리소스 설명  |
|   **metadata** | **String**| **No** | 메타데이터  |
|   **name** | **String**| **No** | 리소스 이름  |
|   **path** | **String**| **Yes** | 리소스 Path  |
|   **priority** | **Integer**| **Yes** | 우선순위  |
|   **resourceId** | **String**| **No** | 리소스 ID  |
|   **resources** | **Set<SearchResourceHierarchy.ResourceHierarchyProtocol>**| **No** | 자식 계층의 리소스 목록  |
|   **uiPath** | **String**| **Yes** | 리소스 UI Path  |







(../Models/SearchResourceHierarchy.ResourceHierarchyProtocol.md)


























## 리소스 연관 역할


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#addAuthorization) | 리소스 역할 연관 관계 추가 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#getAuthorizations) | 리소스 역할 연관 관계 목록 조회 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](#removeAuthorization) | 리소스 역할 연관 관계 삭제 |


<a name="addAuthorization"></a>
### **리소스 역할 연관 관계 추가**
> POST "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
| Request Body | **AddAuthorization.Request** | **AddAuthorization.Request**| **Yes** |  | |






##### AddAuthorization.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **propagation** | **Boolean**| **No** | Root를 제외한 모든 상위 Path에 지정한 역할을 동일하게 적용할지 여부  |
|   **roleId** | **String**| **Yes** | 역할 ID  |











#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="getAuthorizations"></a>
### **리소스 역할 연관 관계 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 









#### Response Body

```json
{
  "authorizations" : [ {
    "resourceId" : "resourceId",
    "roleId" : "roleId",
    "operationId" : "operationId"
  }, {
    "resourceId" : "resourceId",
    "roleId" : "roleId",
    "operationId" : "operationId"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAuthorizations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **authorizations** | **List&lt;ResourceAuthorizationProtocol>**| **No** | 리소스 역할 연관 관계 목록  |

##### ResourceAuthorizationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
|   **resourceId** | **String**| **Yes** | 리소스 ID  |
|   **roleId** | **String**| **Yes** | 역할 Id  |
















<a name="removeAuthorization"></a>
### **리소스 역할 연관 관계 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**resourceId** | **String**| **Yes** | 리소스 ID | 
|  Query |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 
|  Query |**roleId** | **String**| **Yes** | 역할 ID | 











#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 오퍼레이션


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations**](#createOperation) | 오퍼레이션 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#deleteOperation) | 오퍼레이션 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations**](#deleteOperations) | 오퍼레이션 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#getOperation) | 오퍼레이션 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/id**](#getOperationIdByPageable) | 모든 오퍼레이션 ID 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations/search**](#postSearchOperation) | 오퍼레이션 목록 조회(조건/페이징) |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](#updateOperation) | 오퍼레이션 수정 |


<a name="createOperation"></a>
### **오퍼레이션 생성**
> POST "/role/v3.0/appkeys/{appKey}/operations"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateOperation.Request** | **CreateOperation.Request**| **Yes** |  | |





##### CreateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |










#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteOperation"></a>
### **오퍼레이션 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteOperations"></a>
### **오퍼레이션 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/operations"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**operationIds** |  **List&lt;String>**| **Yes** | 오퍼레이션 ID 목록 |


#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getOperation"></a>
### **오퍼레이션 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "operation" : {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  }
}
```



##### GetOperation.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **operation** | **OperationResponseProtocol**| **Yes** | 오퍼레이션       |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | 앱키  |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |















<a name="getOperationIdByPageable"></a>
### **모든 오퍼레이션 ID 조회**
> GET "/role/v3.0/appkeys/{appKey}/operations/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**operationIdPreLike** | **String**| **No** | 오퍼레이션 ID(전방 일치) |
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.operationId,ASC`)|











#### Response Body

```json
{
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "operationIds" : [ "operationIds", "operationIds" ]
}
```



##### GetAllOperationIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operationIds** | **List&lt;String>**| **Yes** | 오퍼레이션 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |











<a name="postSearchOperation"></a>
### **오퍼레이션 목록 조회(조건/페이징)**
> POST "/role/v3.0/appkeys/{appKey}/operations/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.operationId,ASC`)|
| Request Body | **PostSearchOperations.Request** | **PostSearchOperations.Request**| **Yes** |  | |





##### PostSearchOperations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **descriptionLike** | **String**| **No** | 오퍼레이션 설명(부분 일치)  |
|   **operationIdPreLike** | **String**| **No** | 오퍼레이션 ID(전방 일치)  |
|   **operationIds** | **List&lt;String>**| **No** | 오퍼레이션 ID 목록(완전 일치)  |













#### Response Body

```json
{
  "totalItems" : 0,
  "operations" : [ {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  }, {
    "description" : "description",
    "operationId" : "operationId",
    "appKey" : "appKey"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### PostSearchOperations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **operations** | **List&lt;OperationResponseProtocol>**| **Yes** | 오퍼레이션 목록  |
|   **totalItems** | **Long**| **Yes** | 전체 개수  |


##### OperationResponseProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **appKey** | **String**| **No** | 앱키  |
|   **description** | **String**| **No** | 오퍼레이션 설명  |
|   **operationId** | **String**| **Yes** | 오퍼레이션 ID  |
















<a name="updateOperation"></a>
### **오퍼레이션 수정**
> PUT "/role/v3.0/appkeys/{appKey}/operations/{operationId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**operationId** | **String**| **Yes** | 오퍼레이션 ID | 
| Request Body | **UpdateOperation.Request** | **UpdateOperation.Request**| **Yes** |  | |






##### UpdateOperation.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **description** | **String**| **No** | 오퍼레이션 설명  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 조건 속성


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes**](#createAttribute) | 조건 속성 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#deleteAttribute) | 조건 속성 삭제 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes**](#deleteAttributes) | 조건 속성 다건 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#getAttribute) | 조건 속성 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/id**](#searchAttributeIds) | 조건 속성 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/search**](#searchAttributes) | 조건 속성 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](#updateAttribute) | 조건 속성 수정 |


<a name="createAttribute"></a>
### **조건 속성 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **CreateAttribute.Request** | **CreateAttribute.Request**| **Yes** |  | |





##### CreateAttribute.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 조건 속성 설명  |














#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteAttribute"></a>
### **조건 속성 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
|  Query |**forceDelete** | **Boolean**| **No** | 강제 삭제, 기본값(false) |










#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


<a name="deleteAttributes"></a>
### **조건 속성 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes"

#### Parameters

| ParameterType | Name | Type | Required | Description  |
|------------- |------------- | ------------- | ------------- | ------------- |
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 |
| Request Body |**attributeIds** |  **List&lt;String>**| **Yes** | 조건 속성 ID 목록 |
| Request Body |**forceDelete** | **Boolean**| **No** | 강제 삭제, 기본값(false) |

#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```




<a name="getAttribute"></a>
### **조건 속성 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 |
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 









#### Response Body

```json
{
  "attributeInUse" : true,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attribute" : {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  }
}
```



##### GetAttribute.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- |-------------|
|   **attribute** | **AttributeBundleProtocol**| **Yes** | 조건 속성       |
|   **attributeInUse** | **Boolean**| **Yes** | 조건 속성 사용 여부 |

##### AttributeBundleProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 ID  |
|   **description** | **String**| **No** | 조건 속성 설명  |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |












##### AttributeTagProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |






















<a name="searchAttributeIds"></a>
### **조건 속성 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 조건 속성 설명(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |


















#### Response Body

```json
{
  "totalItems" : 0,
  "attributeIds" : [ "attributeIds", "attributeIds" ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeIds.Response


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeIds** | **List&lt;String>**| **Yes** | 조건 속성 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |











<a name="searchAttributes"></a>
### **조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeId,ASC`)|
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**| **Yes** |  | |





##### SearchAttributes.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCodes** | **List&lt;AttributeCreationTypeCode>**| **No** | 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List&lt;AttributeDataTypeCode>**| **No** | 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |
|   **descriptionLike** | **String**| **No** | 조건 속성 설명(부분 일치)  |
|   **roleIdPreLike** | **String**| **No** | 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 역할 ID 목록(완전 일치)  |


















#### Response Body

```json
{
  "totalItems" : 6,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  },
  "attributes" : [ {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  }, {
    "attributeId" : "attributeId",
    "attributeRoleRelations" : [ {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    }, {
      "attributeId" : "attributeId",
      "exposureOrder" : 0,
      "roleId" : "roleId",
      "roleName" : "roleName",
      "description" : "description",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00",
      "roleGroup" : "roleGroup"
    } ],
    "attributeTags" : [ {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    }, {
      "attributeId" : "attributeId",
      "attributeTagId" : "attributeTagId",
      "regYmdt" : "2000-01-23T04:56:07.000+00:00"
    } ],
    "description" : "description",
    "attributeName" : "attributeName"
  } ]
}
```



##### SearchAttributes.Response


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributes** | **List&lt;AttributeBundleProtocol>**| **Yes** | 조건 속성 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeBundleProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeCreationTypeCode** | **String**| **Yes** |   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 ID  |
|   **description** | **String**| **No** | 조건 속성 설명  |





##### AttributeRoleRelationProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |












##### AttributeTagProtocol


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |






















<a name="updateAttribute"></a>
### **조건 속성 수정**
> PUT "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **UpdateAttribute.Request** | **UpdateAttribute.Request**| **Yes** |  | |






##### UpdateAttribute.Request


| Name | Type | Required | Description |
|------------ | ------------- | ------------- | ------------ |
|   **attributeDataTypeCode** | **String**| **Yes** |   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeName** | **String**| **No** | 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록  |
|   **description** | **String**| **No** | 조건 속성 설명  |













#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

## 조건 속성 데이터 타입


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/data-types**](#getAttributeDataType) | 조건 속성 데이터 타입 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/condition/validate**](#validateConditionValues) | 조건 값 유효성 체크 |


<a name="getAttributeDataType"></a>
### **조건 속성 데이터 타입 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/data-types"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 








#### Response Body

```json
{
  "dataTypes" : [ {
    "operators" : [ {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    }, {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    } ],
    "dataType" : "dataType"
  }, {
    "operators" : [ {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    }, {
      "min" : 6,
      "max" : 0,
      "operatorTypeCode" : "operatorTypeCode"
    } ],
    "dataType" : "dataType"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetAttributeDataTypeResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **dataTypes** | **List&lt;GetAttributeDataTypeResponse.AttributeDataTypeProtocol>**| **Yes** | 조건 속성 데이터 타입 목록  |

##### GetAttributeDataTypeResponse.AttributeDataTypeProtocol


| Name | Type | Required | Description         | 
|------------ | ------------- | ------------- |---------------------|
|   **dataType** | **String**| **Yes** | 조건 속성 데이터 타입        |
|   **operators** | **List&lt;GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol>**| **Yes** | 조건 속성 사용 가능한 연산자 목록 |


##### GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **max** | **Integer**| **Yes** | 연산자가 사용할수 있는 값의 최대 개수  |
|   **min** | **Integer**| **Yes** | 연산자가 사용할수 있는 값의 최소 개수  |
|   **operatorTypeCode** | **String**| **Yes** | 연산자  |




















<a name="validateConditionValues"></a>
### **조건 값 유효성 체크**
> POST "/role/v3.0/appkeys/{appKey}/attributes/condition/validate"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **ValidateConditionValuesRequest** | **ValidateConditionValuesRequest**| **Yes** |  | |





##### ValidateConditionValuesRequest


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **conditions** | **List&lt;ConditionProtocol>**| **Yes** | 역할 조건 속성  |

##### ConditionProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**| **Yes** |   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List&lt;String>**| **No** | 조건 속성 값  |















#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```


## 조건 속성 역할 연관 관계


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#createAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](#deleteAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search**](#searchAttributeRoleRelations) | 조건 속성과 연관된 역할 목록 조회 |


<a name="createAttributeRoleRelations"></a>
### **조건 속성과 연관된 역할 다건 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **CreateAttributeRoleRelations.Request** | **CreateAttributeRoleRelations.Request**| **Yes** |  | |






##### CreateAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteAttributeRoleRelations"></a>
### **조건 속성과 연관된 역할 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **DeleteAttributeRoleRelations.Request** | **DeleteAttributeRoleRelations.Request**| **Yes** |  | |






##### DeleteAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelationIds** | **List&lt;String>**| **Yes** | 조건 속성과 연관된 역할 ID 목록  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="searchAttributeRoleRelations"></a>
### **조건 속성과 연관된 역할 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `attribute.id.attributeId,ASC`)|
| Request Body | **SearchAttributeRoleRelations.Request** | **SearchAttributeRoleRelations.Request**| **Yes** |  | |






##### SearchAttributeRoleRelations.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **roleIdPreLike** | **String**| **No** | 조건 속성과 연관된 역할 ID(전방 일치)  |
|   **roleIds** | **List&lt;String>**| **No** | 조건 속성과 연관된 역할 ID 목록(완전 일치)  |
|   **searchRoleOptionCode** | **String**| **No** |   DIRECT_ROLE, INDIRECT_ROLE |













#### Response Body

```json
{
  "attributeRoleRelations" : [ {
    "attributeId" : "attributeId",
    "exposureOrder" : 0,
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "roleGroup" : "roleGroup"
  }, {
    "attributeId" : "attributeId",
    "exposureOrder" : 0,
    "roleId" : "roleId",
    "roleName" : "roleName",
    "description" : "description",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00",
    "roleGroup" : "roleGroup"
  } ],
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeRoleRelations.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeRoleRelations** | **List&lt;AttributeRoleRelationProtocol>**| **Yes** | 조건 속성 연관 관계 Role 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeRoleRelationProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **description** | **String**| **No** | 역할 설명  |
|   **exposureOrder** | **Integer**| **Yes** | 노출 순서  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성과 연관된 역할 ID 생성 일시  |
|   **roleGroup** | **String**| **No** | 역할 그룹  |
|   **roleId** | **String**| **Yes** | 역할 ID  |
|   **roleName** | **String**| **No** | 역할 이름  |



















## 조건 속성 태그


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#createAttributeTags) | 조건 속성 태그 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](#deleteAttributeTags) | 조건 속성 태그 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/id**](#searchAttributeTagIds) | 조건 속성 태그 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/search**](#searchAttributeTags) | 조건 속성 태그 목록 조회 |


<a name="createAttributeTags"></a>
### **조건 속성 태그 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **CreateAttributeTags.Request** | **CreateAttributeTags.Request**| **Yes** |  | |






##### CreateAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="deleteAttributeTags"></a>
### **조건 속성 태그 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Path |**attributeId** | **String**| **Yes** | 조건 속성 ID | 
| Request Body | **DeleteAttributeTags.Request** | **DeleteAttributeTags.Request**| **Yes** |  | |






##### DeleteAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |









#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="searchAttributeTagIds"></a>
### **조건 속성 태그 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/id"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTagIds.Request** | **SearchAttributeTagIds.Request**| **Yes** |  | |





##### SearchAttributeTagIds.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIdPreLike** | **String**| **No** | 조건 속성 태그 ID(전방 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |














#### Response Body

```json
{
  "attributeTagIds" : [ "attributeTagIds", "attributeTagIds" ],
  "totalItems" : 0,
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeTagIds.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTagIds** | **List&lt;String>**| **Yes** | 조건 속성 태그 ID 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |











<a name="searchAttributeTags"></a>
### **조건 속성 태그 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/search"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
|  Query |**page** | **Integer**| **No** | 검색을 원하는 페이지 번호(기본값 1) | 
|  Query |**itemsPerPage** | **Integer**| **No** | 결과를 원하는 페이지별 검색 개수(기본값 10) |  
|  Query |**sort** |  **List&lt;String>**| **No** | 정렬 순서(기본값 `id.attributeTagId,ASC`)|
| Request Body | **SearchAttributeTags.Request** | **SearchAttributeTags.Request**| **Yes** |  | |





##### SearchAttributeTags.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeIdPreLike** | **String**| **No** | 조건 속성 ID(전방 일치)  |
|   **attributeIds** | **List&lt;String>**| **No** | 조건 속성 ID 목록(완전 일치)  |
|   **attributeTagIdPreLike** | **String**| **No** | 조건 속성 태그 ID(전방 일치)  |
|   **attributeTagIds** | **List&lt;String>**| **No** | 조건 속성 태그 ID 목록(완전 일치)  |














#### Response Body

```json
{
  "totalItems" : 0,
  "attributeTags" : [ {
    "attributeId" : "attributeId",
    "attributeTagId" : "attributeTagId",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00"
  }, {
    "attributeId" : "attributeId",
    "attributeTagId" : "attributeTagId",
    "regYmdt" : "2000-01-23T04:56:07.000+00:00"
  } ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### SearchAttributeTags.Response


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeTags** | **List&lt;AttributeTagProtocol>**| **Yes** | 조건 속성 태그 목록  |
|   **totalItems** | **Long**| **Yes** | 역할 전체 개수  |

##### AttributeTagProtocol


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **attributeId** | **String**| **Yes** | 조건 속성 ID  |
|   **attributeTagId** | **String**| **Yes** | 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| **Yes** | 조건 속성 태그 생성 일시  |















## 설정


| Method | HTTP request                                                        | Description                     |
|------------- |---------------------------------------------------------------------|---------------------------------|
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config/cache-evict**](#deleteCache) | ROLE 서비스 서버와 클라이언트 SDK의 캐시 제거 |
| **GET** | [**/role/v3.0/appkeys/{appKey}/config**](#getConfiguration)         | 설정 조회                           |
| **PUT** | [**/role/v3.0/appkeys/{appKey}/config**](#updateConfig)             | 설정 수정                           |


<a name="deleteCache"></a>
### **서버와 클라이언트 SDK의 캐시 제거**
> PUT "/role/v3.0/appkeys/{appKey}/config/cache-evict"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 








#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







<a name="getConfiguration"></a>
### **설정 조회**
> GET "/role/v3.0/appkeys/{appKey}/config"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 








#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



##### GetTenantConfigResponse


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**| **Yes** |   IDENTICAL_PATH, NON_IDENTICAL_PATH |










<a name="updateConfig"></a>
### **설정 수정**
> PUT "/role/v3.0/appkeys/{appKey}/config"

#### Parameters



| ParameterType | Name | Type | Required | Description  | 
|------------- |------------- | ------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| **Yes** | 비밀 키 | 
|  Path |**appKey** | **String**| **Yes** | 앱키 | 
| Request Body | **UpdateConfig.Request** | **UpdateConfig.Request**| **Yes** |  | |





##### UpdateConfig.Request


| Name | Type | Required | Description | 
|------------ | ------------- | ------------- | ------------ |
|   **cacheSize** | **Integer**| **No** | 리소스 ID 기반 인증 캐시 크기  |
|   **cacheSizeByPath** | **Integer**| **No** | 리소스 Hierarchy 조회 캐시 크기  |
|   **cacheSizeTree** | **Integer**| **No** | 리소스 Path 기반 인증 캐시 크기  |
|   **cacheTtl** | **Integer**| **No** |  캐시 데이터 유지 시간(초 단위) |
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**| **No** |   IDENTICAL_PATH, NON_IDENTICAL_PATH |



#### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```
