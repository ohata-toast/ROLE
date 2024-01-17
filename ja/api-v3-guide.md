# Application Service > ROLE > API v3 가이드

> Role 상품을 이용하여 권한을 체크하기 위해서는
> RESTFUL API 를 호출하거나, Client SDK 를 이용하여야 한다.

## AppKey & SecretKey

RESTFUL API 와 Client SDK 를 사용하려면 AppKey 와 Secret Key 가 필요하다.
[CONSOLE] 의 좌측 상단에서 발급된 Key 정보를 확인 할 수 있다.

![[그림 1] AppKey & SecretKey 확인](http://static.toastoven.net/prod_role/role_60.png)
<center>[그림 1] AppKey & SecretKey 확인</center>

## RESTFUL API 가이드

### Common Response Body

모든 API 요청에 대해 HTTP 응답 코드는 200 으로 응답한다.
자세한 응답 결과는 Response Body 의 header 항목을 참고한다.

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

| Key                  | 	Type    | 	Description                           |
|----------------------|----------|----------------------------------------|
| header               | 	Object  | 	응답헤더                                  |
| header.isSuccessful  | 	boolean | 	성공 여부                                 |
| header.resultCode    | 	int     | 	응답 코드. 성공 시 0, 실패 시 에러코드 반환           |
| header.resultMessage | 	String  | 	응답 메시지. 성공 시 "SUCCESS", 실패 시 에러메시지 반환 |
| cache                | Object   |  캐시                                       |
| cache.cacheFlushTime | String   | 캐시 삭제 시간 | 
| cache.size | int      | 리소스 ID 기반 인증 캐시 크기 |
| cache.sizeByPath | int      | 리소스 Path 기반 인증 캐시 크기 |
| cache.sizeTree | int      | 리소스 Hierarchy 조회 캐시 크기 |
| cache.ttl | int      | 캐시 데이터 유지 시간 (초 단위) |

# 사용자


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users**](UserApi.md#createUsers) | 사용자 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](UserApi.md#deleteUser) | 사용자 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/id**](UserApi.md#getAllUsers) | 모든 사용자 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](UserApi.md#getUser) | 사용자 정보 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/histories**](UserApi.md#getUserRoleHistories) | 사용자에게 할당된 역할의 변경 내역 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/search**](UserApi.md#getUsers) | 사용자 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/users/{userId}**](UserApi.md#updateUser) | 사용자 수정 |


<a name="createUsers"></a>
## **사용자 생성**
> POST "/role/v3.0/appkeys/{appKey}/users"




| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateUserRequest** | **CreateUserRequest**|  | |



#### CreateUserRequest


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **users** | **List\<CreateUserRequest.UserProtocol\>**| 사용자 목록  |

#### CreateUserRequest.UserProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 사용자 설명  |
|   **roleRelations** | **List\<UserRoleRelationProtocol\>**| 사용자 연관 역할  |
|   **userId** | **String**| 사용자 ID  |


#### UserRoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionProtocol\>**| 역할 조건 속성  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleId** | **String**| 역할 ID  |
|   **scopeId** | **String**| 범위 ID  |

#### ConditionProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |


### Response Body

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
## **사용자 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/users/{userId}"

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |



### Response Body

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
## **모든 사용자 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/users/id"

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchUser.Request** | **SearchUser.Request**|  | |



#### SearchUser.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **descriptionLike** | **String**| 사용자 설명 (부분 일치)  |
|   **needRoleRelations** | **Boolean**| 응답 시 역할 연관관계 포함 여부  |
|   **needRoleTags** | **Boolean**| 응답 시 역할 연관관계 포함 시 역할 태그 포함 여부  |
|   **roleIdPreLike** | **String**| 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 역할 ID 목록 (완전 일치)  |
|   **scopeIdPreLike** | **String**| 범위 ID (전방 일치)  |
|   **scopeIds** | **List\<String\>**| 범위 ID 목록 (완전 일치)  |
|   **searchRoleOptionCode** | **String**|   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| 사용자 ID (전방 일치)  |
|   **userIds** | **List\<String\>**| 사용자 ID 목록 (완전 일치)  |




### Response Body

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



#### SearchAllUser.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **totalItems** | **Long**| 전체 개수  |
|   **userIds** | **List\<String\>**| 사용자 목록  |



<a name="getUser"></a>
## **사용자 정보 조회**
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}"

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |
|  Query |**searchRoleOptionCode** | **String**| 접근 가능한 역할 목록 검색 방식 |  [enum: DIRECT_ROLE, INDIRECT_ROLE] |
|  Query |**roleIds** | **List\<String\>**| 연관관계 역할 ID |  |
|  Query |**scopeIds** | **List\<String\>**| 연관관계 범위 ID |  |








### Response Body

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



#### GetUser.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **user** | **UserBundleProtocol**|   |




#### UserBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 설명  |
|   **regYmdt** | **Date**| 사용자 생성일시  |
|   **roleRelations** | **List\<UserBundleProtocol.UserRoleRelationBundleProtocol\>**| 사용자에 할당된 역할 목록  |
|   **userId** | **String**| 사용자 ID  |



#### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionBundleProtocol\>**| 역할 조건 속성  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regYmdt** | **Date**| 등록일시  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |
|   **roleTags** | **List\<UserBundleProtocol.RoleTagProtocol\>**| 역할 태그 목록  |
|   **scopeId** | **String**| 범위 ID  |

#### ConditionBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeProtocol**|   |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |
























#### UserBundleProtocol.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |























<a name="getUserRoleHistories"></a>
## **사용자에게 할당된 역할의 변경 내역 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/users/{userId}/histories"

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |
|  Query |**roleId** | **String**| 역할 ID |  |
|  Query |**scopeId** | **String**| 범위 ID |  |
|  Query |**fromDateTime** | **Date**| 변경 시작일시 |  |
|  Query |**toDateTime** | **Date**| 변경 종료일시 |  |
|  Query |**historyType** | **List\<String\>**| 변경 유형 |  [enum: USER_ADD, USER_REMOVE, ADD, REMOVE] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |










### Response Body

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



#### GetUserHistory.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **totalItems** | **Long**| 전체 개수  |
|   **userHistory** | **List\<UserHistoryProtocol\>**| 사용자 변경 내역 목록  |










#### UserHistoryProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **command** | **String**|   USER_ADD, USER_REMOVE, ADD, REMOVE |
|   **conditions** | **List\<ConditionBundleProtocol\>**| 역할 조건 속성  |
|   **executionTime** | **Date**| 변경 일시  |
|   **operatorUuid** | **String**| 작업자 UUID  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleId** | **String**| 역할 ID  |
|   **scopeId** | **String**| 범위 ID  |
|   **userHistorySeq** | **Long**| 사용자 변경 이력 일렬번호  |
|   **userId** | **String**| 사용자 ID  |


#### ConditionBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeProtocol**|   |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |



































<a name="getUsers"></a>
## **사용자 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/users/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchUser.Request** | **SearchUser.Request**|  | |



#### SearchUser.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **descriptionLike** | **String**| 사용자 설명 (부분 일치)  |
|   **needRoleRelations** | **Boolean**| 응답 시 역할 연관관계 포함 여부  |
|   **needRoleTags** | **Boolean**| 응답 시 역할 연관관계 포함 시 역할 태그 포함 여부  |
|   **roleIdPreLike** | **String**| 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 역할 ID 목록 (완전 일치)  |
|   **scopeIdPreLike** | **String**| 범위 ID (전방 일치)  |
|   **scopeIds** | **List\<String\>**| 범위 ID 목록 (완전 일치)  |
|   **searchRoleOptionCode** | **String**|   DIRECT_ROLE, INDIRECT_ROLE |
|   **userIdPreLike** | **String**| 사용자 ID (전방 일치)  |
|   **userIds** | **List\<String\>**| 사용자 ID 목록 (완전 일치)  |



















### Response Body

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



#### SearchUser.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **totalItems** | **Long**| 전체 개수  |
|   **users** | **List\<UserBundleProtocol\>**| 사용자 목록  |










#### UserBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 설명  |
|   **regYmdt** | **Date**| 사용자 생성일시  |
|   **roleRelations** | **List\<UserBundleProtocol.UserRoleRelationBundleProtocol\>**| 사용자에 할당된 역할 목록  |
|   **userId** | **String**| 사용자 ID  |



#### UserBundleProtocol.UserRoleRelationBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionBundleProtocol\>**| 역할 조건 속성  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regYmdt** | **Date**| 등록일시  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |
|   **roleTags** | **List\<UserBundleProtocol.RoleTagProtocol\>**| 역할 태그 목록  |
|   **scopeId** | **String**| 범위 ID  |

#### ConditionBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeProtocol**|   |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |
























#### UserBundleProtocol.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |























<a name="updateUser"></a>
## **사용자 수정**
> PUT "/role/v3.0/appkeys/{appKey}/users/{userId}"

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |
| Request Body | **PutUserRequest** | **PutUserRequest**|  | |




#### PutUserRequest


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **user** | **PutUserRequest.UserProtocol**|   |

#### PutUserRequest.UserProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 사용자 설명  |
|   **roleRelations** | **List\<UserRoleRelationProtocol\>**| 사용자 연관 역할  |


#### UserRoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionProtocol\>**| 역할 조건 속성  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleId** | **String**|   |
|   **scopeId** | **String**|   |

#### ConditionProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |



























### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```






# 사용자 인증


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources**](UserAuthorizationApi.md#checkResource) | 사용자가 리소스에 접근 권한이 있는지 검사 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles**](UserAuthorizationApi.md#checkRole) | 사용자가 역할에 대한 접근 권한이 있는지 검사 |


<a name="checkResource"></a>
## **사용자가 리소스에 접근 권한이 있는지 검사**
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/resources"


    resourceId 혹은 resourcePath 중 반드시 하나의 값은 존재해야 한다.

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |
| Request Body | **PostAuthorizationResource.Request** | **PostAuthorizationResource.Request**|  | |




#### PostAuthorizationResource.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resources** | **List\<PostAuthorizationResource.ResourceProtocol\>**|   |

#### PostAuthorizationResource.ResourceProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<PostAuthorizationResource.AttributeProtocol\>**| 조건 속성 ID  |
|   **authRequestId** | **String**| 요청 인증 식별키  |
|   **operationId** | **String**| 오퍼레이션 ID  |
|   **resourceId** | **String**| 리소스 ID  |
|   **resourcePath** | **String**| 리소스 Path  |
|   **scopeId** | **String**| 범위 ID  |

#### PostAuthorizationResource.AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeValue** | **String**| 조건 속성 값  |
























### Response Body

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



#### PostAuthorizationResource.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **authorizations** | **List\<PostAuthorizationResource.AuthorizationWithResourceProtocol\>**| 권한 체크 결과 목록  |

#### PostAuthorizationResource.AuthorizationWithResourceProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<PostAuthorizationResource.AttributeProtocol\>**| 조건 속성 ID  |
|   **authRequestId** | **String**| 요청 인증 식별키  |
|   **operationId** | **String**| 오퍼레이션 ID  |
|   **permission** | **Boolean**| 권한 여부  |
|   **resourceId** | **String**| 리소스 ID  |
|   **resourcePath** | **String**| 리소스 Path  |
|   **scopeId** | **String**| 범위 ID  |

#### PostAuthorizationResource.AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeValue** | **String**| 조건 속성 값  |
































<a name="checkRole"></a>
## **사용자가 역할에 대한 접근 권한이 있는지 검사**
> POST "/role/v3.0/appkeys/{appKey}/users/{userId}/authorizations/roles"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**userId** | **String**| 사용자 ID | [default to null] |
| Request Body | **PostAuthorizationRole.Request** | **PostAuthorizationRole.Request**|  | |




#### PostAuthorizationRole.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roles** | **List\<PostAuthorizationRole.AuthRoleProtocol\>**| 인증 요청 목록  |

#### PostAuthorizationRole.AuthRoleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<PostAuthorizationRole.AttributeProtocol\>**| 조건 속성  |
|   **authRequestId** | **String**| 인증 요청 식별키  |
|   **roleId** | **String**| 역할 ID  |
|   **scopeId** | **String**| 범위 ID  |

#### PostAuthorizationRole.AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeValue** | **String**| 조건 속성 값  |






















### Response Body

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



#### PostAuthorizationRole.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **authorizations** | **List\<PostAuthorizationRole.AuthorizationProtocol\>**| 권한 체크 결과 목록  |

#### PostAuthorizationRole.AuthorizationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<PostAuthorizationRole.AttributeProtocol\>**| 조건 속성 ID  |
|   **authRequestId** | **String**| 요청 인증 식별키  |
|   **permission** | **Boolean**| 권한 여부  |
|   **roleId** | **String**| 역할 ID  |
|   **scopeId** | **String**| 범위 ID  |

#### PostAuthorizationRole.AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeValue** | **String**| 조건 속성 값  |





























# 역할


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles**](RoleApi.md#createRole) | 역할 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](RoleApi.md#deleteRole) | 역할 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable**](RoleApi.md#getDeniable) | 역할 사용여부 DENY(미사용)로 변경가능 여부 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](RoleApi.md#getRole) | 역할 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/id**](RoleApi.md#searchAllRoleIds) | 모든 역할 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search**](RoleApi.md#searchAttributesByRoleId) | 역할에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/roles/search**](RoleApi.md#searchRoles) | 역할 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/roles/{roleId}**](RoleApi.md#updateRole) | 역할 수정 |


<a name="createRole"></a>
## **역할 생성**
> POST "/role/v3.0/appkeys/{appKey}/roles"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateRoleRequest** | **CreateRoleRequest**|  | |



#### CreateRoleRequest


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **role** | **RoleProtocol**|   |
|   **roleRelations** | **List\<CreateRoleRequest.RoleRelationProtocol\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **roleTags** | **List\<CreateRoleRequest.RoleTagProtocol\>**| 역할 태그 목록  |

#### RoleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |










#### CreateRoleRequest.RoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionProtocol\>**| 역할 조건 속성  |
|   **relatedRoleId** | **String**| 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |

#### ConditionProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |














#### CreateRoleRequest.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |














### Response Body

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
## **역할 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/roles/{roleId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**roleId** | **String**| 역할 ID | [default to null] |








### Response Body

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
## **역할 사용여부 DENY(미사용)로 변경가능 여부**
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}/deniable"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**roleId** | **String**| 역할 ID | [default to null] |








### Response Body

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



#### GetDeniableResponse


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **deniable** | **Boolean**| 역할 사용여부 DENY(미사용)로 변경가능 여부  |

















<a name="getRole"></a>
## **역할 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/{roleId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**roleId** | **String**| 역할 ID | [default to null] |








### Response Body

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



#### GetRoleResponse


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **role** | **RoleBundleProtocol**|   |









#### RoleBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **appKey** | **String**| 앱키  |
|   **attributes** | **List\<AttributeProtocol\>**| 조건 속성 목록  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regDateTime** | **Date**| 역할 생성일시  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |
|   **roleRelations** | **List\<RoleBundleProtocol.RoleRelationProtocol\>**| 연관관계 역할 목록  |
|   **roleTags** | **List\<RoleBundleProtocol.RoleTagProtocol\>**| 역할 태그 목록  |


#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |
















#### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionBundleProtocol\>**| 역할 조건 속성  |
|   **description** | **String**| 역할 설명  |
|   **regDateTime** | **Date**| 역할 생성일시  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |

#### ConditionBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeProtocol**|   |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |



























#### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |

















<a name="searchAllRoleIds"></a>
## **모든 역할 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**roleIdPreLike** | **String**| 역할 ID (전방 일치) |  |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |








### Response Body

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



#### GetAllRoleIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleIds** | **List\<String\>**| 역할 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















<a name="searchAttributesByRoleId"></a>
## **역할에서 설정 가능한 모든 조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/roles/{roleId}/attributes/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**roleId** | **String**| 역할 ID | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchRoleAttributes.Request** | **SearchRoleAttributes.Request**|  | |




#### SearchRoleAttributes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeNameLike** | **String**| 조건 속성 이름 (부분 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |












### Response Body

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



#### SearchRoleAttributes.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<AttributeProtocol\>**| 역할에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| 전체 개수  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |


























<a name="searchRoles"></a>
## **역할 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/roles/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **GetRoles.Request** | **GetRoles.Request**|  | |



#### GetRoles.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |
|   **descriptionLike** | **String**| 역할 설명 (부분 일치)  |
|   **needAttributes** | **Boolean**| 응답 시 조건 속성 정보 포함 여부  |
|   **needRoleRelations** | **Boolean**| 응답 시 역할 연관관계 ID 목록 포함 여부  |
|   **needRoleTags** | **Boolean**| 응답 시 역할 태그 ID 목록 포함 여부  |
|   **relatedRoleIds** | **List\<String\>**| 연관관계 역할 ID 목록 (완전 일치)  |
|   **roleGroupLike** | **String**| 역할 그룹 (부분 일치)  |
|   **roleIdPreLike** | **String**| 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 역할 ID 목록 (완전 일치)  |
|   **roleNameLike** | **String**| 역할 이름 (부분 일치)  |
|   **roleTagIdExpr** | **String**| 역할 태그 조건 (구분자 &#39;;&#39;:OR, &#39;,&#39;:AND)  |
|   **roleTagIds** | **List\<String\>**| 역할 태그 ID 목록 (완전 일치)  |






















### Response Body

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



#### GetRoles.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roles** | **List\<RoleBundleProtocol\>**| 역할 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |









#### RoleBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **appKey** | **String**| 앱키  |
|   **attributes** | **List\<AttributeProtocol\>**| 조건 속성 목록  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regDateTime** | **Date**| 역할 생성일시  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |
|   **roleRelations** | **List\<RoleBundleProtocol.RoleRelationProtocol\>**| 연관관계 역할 목록  |
|   **roleTags** | **List\<RoleBundleProtocol.RoleTagProtocol\>**| 역할 태그 목록  |


#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |
















#### RoleBundleProtocol.RoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionBundleProtocol\>**| 역할 조건 속성  |
|   **description** | **String**| 역할 설명  |
|   **regDateTime** | **Date**| 역할 생성일시  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |

#### ConditionBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeProtocol**|   |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |



























#### RoleBundleProtocol.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |


















<a name="updateRole"></a>
## **역할 수정**
> PUT "/role/v3.0/appkeys/{appKey}/roles/{roleId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**roleId** | **String**| 역할 ID | [default to null] |
| Request Body | **UpdateRoleRequest** | **UpdateRoleRequest**|  | |




#### UpdateRoleRequest


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **role** | **RoleMetadataProtocol**|   |
|   **roleRelations** | **List\<UpdateRoleRequest.RoleRelationProtocol\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **roleTags** | **List\<UpdateRoleRequest.RoleTagProtocol\>**| 역할 태그 목록  |

#### RoleMetadataProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleName** | **String**| 역할 이름  |









#### UpdateRoleRequest.RoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionProtocol\>**| 역할 조건 속성  |
|   **relatedRoleId** | **String**| 조건 속성과 연관된 역할 ID  |
|   **roleApplyPolicyCode** | **String**|   ALLOW, DENY |

#### ConditionProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |














#### UpdateRoleRequest.RoleTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagId** | **String**| 역할 태그 ID  |














### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```






# 역할 태그


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **GET** |[**/role/v3.0/appkeys/{appKey}/roles/tags/id**](RoleTagApi.md#getAllRoleTagIds) | 모든 역할 태그 ID 목록 조회 |


<a name="getAllRoleTagIds"></a>
## **모든 역할 태그 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/roles/tags/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**roleTagIdPreLike** | **String**| 역할 태그 ID (전방 일치) |  |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |








### Response Body

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



#### GetAllRoleTagIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleTagIds** | **List\<String\>**| 역할 태그 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















# 범위


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](ScopeApi.md#createScope) | 범위 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#deleteScope) | 범위 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](ScopeApi.md#getAllScopeIds) | 모든 범위 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#getScope) | 범위 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](ScopeApi.md#postSearchScopes) | 범위 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#updateScope) | 범위 수정 |


<a name="createScope"></a>
## **범위 생성**
> POST "/role/v3.0/appkeys/{appKey}/scopes"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateScope.Request** | **CreateScope.Request**|  | |



#### CreateScope.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |












### Response Body

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
## **범위 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |








### Response Body

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
## **모든 범위 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Query |**scopeIdPreLike** | **String**| 범위 ID (전방 일치) |  |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |








### Response Body

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



#### GetAllScopeIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scopeIds** | **List\<String\>**| 범위 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















<a name="getScope"></a>
## **범위 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |








### Response Body

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



#### GetScope.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scope** | **ScopeProtocol**|   |









#### ScopeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |














<a name="postSearchScopes"></a>
## **범위 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**|  | |



#### PostSearchScopes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **descriptionLike** | **String**| 범위 설명 (부분 일치)  |
|   **scopeIdPreLike** | **String**| 범위 ID (전방 일치)  |
|   **scopeIds** | **List\<String\>**| 범위 ID 목록 (완전 일치)  |












### Response Body

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



#### PostSearchScopes.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scopes** | **List\<ScopeProtocol\>**| 범위 목록  |
|   **totalItems** | **Long**| 범위 총 개수  |









#### ScopeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |















<a name="updateScope"></a>
## **범위 수정**
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**|  | |




#### UpdateScope.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 설명  |










### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







# Scope


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes**](ScopeApi.md#createScope) | 범위 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#deleteScope) | 범위 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/id**](ScopeApi.md#getAllScopeIds) | 모든 범위 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#getScope) | 범위 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/scopes/search**](ScopeApi.md#postSearchScopes) | 범위 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/scopes/{scopeId}**](ScopeApi.md#updateScope) | 범위 수정 |


<a name="createScope"></a>
## **범위 생성**
> POST "/role/v3.0/appkeys/{appKey}/scopes"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateScope.Request** | **CreateScope.Request**|  | |



#### CreateScope.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |











### Response Body

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
## **범위 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |








### Response Body

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
## **모든 범위 ID 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Query |**scopeIdPreLike** | **String**| 범위 ID (전방 일치) |  |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |








### Response Body

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



#### GetAllScopeIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scopeIds** | **List\<String\>**| 범위 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















<a name="getScope"></a>
## **범위 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |








### Response Body

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



#### GetScope.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scope** | **ScopeProtocol**|   |









#### ScopeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |














<a name="postSearchScopes"></a>
## **범위 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/scopes/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **PostSearchScopes.Request** | **PostSearchScopes.Request**|  | |



#### PostSearchScopes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **descriptionLike** | **String**| 범위 설명 (부분 일치)  |
|   **scopeIdPreLike** | **String**| 범위 ID (전방 일치)  |
|   **scopeIds** | **List\<String\>**| 범위 ID 목록 (완전 일치)  |












### Response Body

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



#### PostSearchScopes.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **scopes** | **List\<ScopeProtocol\>**| 범위 목록  |
|   **totalItems** | **Long**| 범위 총 개수  |









#### ScopeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 범위 설명  |
|   **scopeId** | **String**| 범위 ID  |















<a name="updateScope"></a>
## **범위 수정**
> PUT "/role/v3.0/appkeys/{appKey}/scopes/{scopeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**scopeId** | **String**|  | [default to null] |
| Request Body | **UpdateScope.Request** | **UpdateScope.Request**|  | |




#### UpdateScope.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 설명  |










### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







# 리소스


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources**](ResourceApi.md#createResource) | 리소스 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](ResourceApi.md#deleteResource) | 리소스 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](ResourceApi.md#getResource) | 리소스 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/id**](ResourceApi.md#getResourceIds) | 리소스 ID 목록 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources**](ResourceApi.md#getResources) | 리소스 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/attributes/search**](ResourceApi.md#searchAttributesByResource) | 리소스에서 설정 가능한 모든 조건 속성 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/search**](ResourceApi.md#searchResources) | 리소스 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}**](ResourceApi.md#updateResource) | 리소스 수정 |


<a name="createResource"></a>
## **리소스 생성**
> POST "/role/v3.0/appkeys/{appKey}/resources"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateResource.Request** | **CreateResource.Request**|  | |



#### CreateResource.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 리소스 설명  |
|   **metadata** | **String**| 메타데이터  |
|   **name** | **String**| 리소스 이름  |
|   **path** | **String**| 리소스 Path  |
|   **priority** | **Integer**| 우선순위  |
|   **resourceId** | **String**| 리소스 ID  |
|   **uiPath** | **String**| 리소스 UI Path  |
















### Response Body

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
## **리소스 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |








### Response Body

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
## **리소스 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |








### Response Body

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



#### GetResource.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resource** | **ResourceProtocol**|   |









#### ResourceProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 리소스 설명  |
|   **metadata** | **String**| 메타데이터  |
|   **name** | **String**| 리소스 이름  |
|   **path** | **String**| 리소스 Path  |
|   **priority** | **Integer**| 우선순위  |
|   **resourceId** | **String**| 리소스 ID  |
|   **uiPath** | **String**| 리소스 UI Path  |



















<a name="getResourceIds"></a>
## **리소스 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **GetAllResourceIds.Request** | **GetAllResourceIds.Request**|  | |



#### GetAllResourceIds.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationIds** | **List\<String\>**|   |
|   **resourceIdPreLike** | **String**|   |
|   **roleIds** | **List\<String\>**|   |
|   **userIds** | **List\<String\>**|   |













### Response Body

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



#### GetAllResourceIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resourceIds** | **List\<String\>**| 리소스 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















<a name="getResources"></a>
## **리소스 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**userId** | **String**| 리소스에 접근 가능한 사용자 ID |  |
|  Query |**roleId** | **String**| 리소스에 부여된 역할 ID |  |
|  Query |**operationId** | **String**| 리소스에 부여된 Operation ID |  |










### Response Body

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



#### GetResources.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resources** | **List\<ResourceProtocol\>**| 리소스 목록  |









#### ResourceProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 리소스 설명  |
|   **metadata** | **String**| 메타데이터  |
|   **name** | **String**| 리소스 이름  |
|   **path** | **String**| 리소스 Path  |
|   **priority** | **Integer**| 우선순위  |
|   **resourceId** | **String**| 리소스 ID  |
|   **uiPath** | **String**| 리소스 UI Path  |



















<a name="searchAttributesByResource"></a>
## **리소스에서 설정 가능한 모든 조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/attributes/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchResourceAttributes.Request** | **SearchResourceAttributes.Request**|  | |



#### SearchResourceAttributes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationId** | **String**| 오퍼레이션 ID  |
|   **resourceId** | **String**| 리소스 ID, ID 와 Path 가 둘다 있을 경우 ID 기준으로만 제공  |
|   **resourcePath** | **String**| 리소스 Path  |












### Response Body

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



#### SearchResourceAttributes.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<AttributeProtocol\>**| 리소스에 부여 가능한 조건 속성 목록  |
|   **totalItems** | **Long**| 전체 개수  |

#### AttributeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **description** | **String**| 조건 속성 설명  |


























<a name="searchResources"></a>
## **리소스 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/resources/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **PostSearchResources.Request** | **PostSearchResources.Request**|  | |



#### PostSearchResources.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationIds** | **List\<String\>**| 리소스에 부여된 Operation ID 목록  |
|   **resourceIdPreLike** | **String**| 리소스 ID (전방 일치)  |
|   **resourceIds** | **List\<String\>**| 리소스 ID 목록  |
|   **resourcePath** | **String**| 리소스 Path (완전 일치)  |
|   **resourcePathLike** | **String**| 리소스 Path (전방 일치)  |
|   **resourcePaths** | **List\<String\>**| 리소스 Path 목록 (완전 일치)  |
|   **resourceUiPath** | **String**| 리소스 UI Path (완전 일치)  |
|   **resourceUiPaths** | **List\<String\>**| 리소스 UI Path 목록 (완전 일치)  |
|   **roleIds** | **List\<String\>**| 리소스에 부여된 역할 ID 목록  |
|   **scopeIds** | **List\<String\>**| 리소스에 접근 가능한 범위 ID 목록  |
|   **searchRoleOptionCode** | **String**| 접근 가능한 역할 목록 검색 방식  DIRECT_ROLE, INDIRECT_ROLE |
|   **userIds** | **List\<String\>**| 리소스에 접근 가능한 사용자 ID 목록  |





















### Response Body

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



#### PostSearchResources.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resources** | **List\<ResourceProtocol\>**| 리소스 목록  |
|   **totalItems** | **Long**| 전체 개수  |









#### ResourceProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 리소스 설명  |
|   **metadata** | **String**| 메타데이터  |
|   **name** | **String**| 리소스 이름  |
|   **path** | **String**| 리소스 Path  |
|   **priority** | **Integer**| 우선순위  |
|   **resourceId** | **String**| 리소스 ID  |
|   **uiPath** | **String**| 리소스 UI Path  |




















<a name="updateResource"></a>
## **리소스 수정**
> PUT "/role/v3.0/appkeys/{appKey}/resources/{resourceId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |
| Request Body | **UpdateResource.Request** | **UpdateResource.Request**|  | |




#### UpdateResource.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 리소스 설명  |
|   **metadata** | **String**| 메타데이터  |
|   **name** | **String**| 리소스 이름  |
|   **newResourceId** | **String**| 변경할 리소스 ID  |
|   **path** | **String**| 리소스 Path  |
|   **priority** | **Integer**| 우선순위  |
|   **uiPath** | **String**| 리소스 UI Path  |
















### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







# ResourceRoleRelation


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](ResourceRoleRelationApi.md#addAuthorization) | 리소스 역할 연관관계 추가 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](ResourceRoleRelationApi.md#getAuthorizations) | 리소스 역할 연관관계 목록 조회 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations**](ResourceRoleRelationApi.md#removeAuthorization) | 리소스 역할 연관관계 삭제 |


<a name="addAuthorization"></a>
## **리소스 역할 연관관계 추가**
> POST "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |
| Request Body | **AddAuthorization.Request** | **AddAuthorization.Request**|  | |




#### AddAuthorization.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationId** | **String**| 오퍼레이션 ID  |
|   **propagation** | **Boolean**| Root를 제외한 모든 상위 Path에 지정한 역할을 동일하게 적용할지 여부  |
|   **roleId** | **String**| 역할 ID  |












### Response Body

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
## **리소스 역할 연관관계 목록 조회**
> GET "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |








### Response Body

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



#### GetAuthorizations.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **authorizations** | **List\<ResourceAuthorizationProtocol\>**| 리소스 역할 연관관계 목록  |

#### ResourceAuthorizationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationId** | **String**| 오퍼레이션 ID  |
|   **resourceId** | **String**| 리소스 ID  |
|   **roleId** | **String**| 역할 Id  |























<a name="removeAuthorization"></a>
## **리소스 역할 연관관계 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/resources/{resourceId}/authorizations"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**resourceId** | **String**| 리소스 ID | [default to null] |
|  Query |**operationId** | **String**| 오퍼레이션 ID | [default to null] |
|  Query |**roleId** | **String**| 역할 ID | [default to null] |










### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```







# 오퍼레이션


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations**](OperationApi.md#createOperation) | 오퍼레이션 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](OperationApi.md#deleteOperation) | 오퍼레이션 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](OperationApi.md#getOperation) | 오퍼레이션 단건 조회 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/operations/id**](OperationApi.md#getOperationIdByPageable) | 모든 오퍼레이션 ID 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/operations/search**](OperationApi.md#postSearchOperation) | 오퍼레이션 목록 조회(조건/페이징) |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/operations/{operationId}**](OperationApi.md#updateOperation) | 오퍼레이션 수정 |


<a name="createOperation"></a>
## **오퍼레이션 생성**
> POST "/role/v3.0/appkeys/{appKey}/operations"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateOperation.Request** | **CreateOperation.Request**|  | |



#### CreateOperation.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 오퍼레이션 설명  |
|   **operationId** | **String**| 오퍼레이션 ID  |











### Response Body

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
## **오퍼레이션 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/operations/{operationId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**operationId** | **String**|  | [default to null] |








### Response Body

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
## **오퍼레이션 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/operations/{operationId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**operationId** | **String**|  | [default to null] |








### Response Body

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



#### GetOperation.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operation** | **OperationResponseProtocol**|   |









#### OperationResponseProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **appKey** | **String**| 앱키  |
|   **description** | **String**| 오퍼레이션 설명  |
|   **operationId** | **String**| 오퍼레이션 ID  |















<a name="getOperationIdByPageable"></a>
## **모든 오퍼레이션 ID 조회**
> GET "/role/v3.0/appkeys/{appKey}/operations/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Query |**operationIdPreLike** | **String**| 오퍼레이션 ID (전방 일치) |  |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |








### Response Body

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



#### GetAllOperationIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operationIds** | **List\<String\>**| 오퍼레이션 ID 목록  |
|   **totalItems** | **Long**| 전체 개수  |


















<a name="postSearchOperation"></a>
## **오퍼레이션 목록 조회(조건/페이징)**
> POST "/role/v3.0/appkeys/{appKey}/operations/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **PostSearchOperations.Request** | **PostSearchOperations.Request**|  | |



#### PostSearchOperations.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **descriptionLike** | **String**| 오퍼레이션 설명 (부분 일치)  |
|   **operationIdPreLike** | **String**| 오퍼레이션 ID (전방 일치)  |
|   **operationIds** | **List\<String\>**| 오퍼레이션 ID 목록 (완전 일치)  |












### Response Body

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



#### PostSearchOperations.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **operations** | **List\<OperationResponseProtocol\>**| 오퍼레이션 목록  |
|   **totalItems** | **Long**| 전체 개수  |









#### OperationResponseProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **appKey** | **String**| 앱키  |
|   **description** | **String**| 오퍼레이션 설명  |
|   **operationId** | **String**| 오퍼레이션 ID  |
















<a name="updateOperation"></a>
## **오퍼레이션 수정**
> PUT "/role/v3.0/appkeys/{appKey}/operations/{operationId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**|  | [default to null] |
|  Path |**operationId** | **String**|  | [default to null] |
| Request Body | **UpdateOperation.Request** | **UpdateOperation.Request**|  | |




#### UpdateOperation.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **description** | **String**| 오퍼레이션 설명  |










### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```






# Attribute


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes**](AttributeApi.md#createAttribute) | 조건 속성 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](AttributeApi.md#deleteAttribute) | 조건 속성 삭제 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](AttributeApi.md#getAttribute) | 조건 속성 단건 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/id**](AttributeApi.md#searchAttributeIds) | 조건 속성 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/search**](AttributeApi.md#searchAttributes) | 조건 속성 목록 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}**](AttributeApi.md#updateAttribute) | 조건 속성 수정 |


<a name="createAttribute"></a>
## **조건 속성 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **CreateAttribute.Request** | **CreateAttribute.Request**|  | |



#### CreateAttribute.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List\<String\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록  |
|   **description** | **String**| 조건 속성 설명  |















### Response Body

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
## **조건 속성 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
|  Query |**forceDelete** | **Boolean**| 강제 삭제, 기본값(false) |  |









### Response Body

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
## **조건 속성 단건 조회**
> GET "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |








### Response Body

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



#### GetAttribute.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attribute** | **AttributeBundleProtocol**|   |
|   **attributeInUse** | **Boolean**| 조건 속성 사용 여부  |

#### AttributeBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **attributeRoleRelations** | **List\<AttributeRoleRelationProtocol\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List\<AttributeTagProtocol\>**| 조건 속성 태그 ID  |
|   **description** | **String**| 조건 속성 설명  |





#### AttributeRoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regYmdt** | **Date**| 조건 속성과 연관된 역할 ID 생성일시  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |












#### AttributeTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeTagId** | **String**| 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| 조건 속성 태그 생성일시  |





























<a name="searchAttributeIds"></a>
## **조건 속성 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/id"


    사용자 정의 조건 속성 및 공통 조건 속성의 ID 목록을 제공한다.

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**|  | |



#### SearchAttributes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCodes** | **List\<AttributeCreationTypeCode\>**| 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List\<AttributeDataTypeCode\>**| 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| 조건 속성 ID (전방 일치)  |
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |
|   **descriptionLike** | **String**| 조건 속성 설명 (부분 일치)  |
|   **roleIdPreLike** | **String**| 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 역할 ID 목록 (완전 일치)  |















### Response Body

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



#### SearchAttributeIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |


















<a name="searchAttributes"></a>
## **조건 속성 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/search"


    사용자 정의 조건 속성 및 공통 조건 속성의 목록을 제공한다.

### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchAttributes.Request** | **SearchAttributes.Request**|  | |



#### SearchAttributes.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCodes** | **List\<AttributeCreationTypeCode\>**| 조건 속성 생성 타입 목록  |
|   **attributeDataTypeCodes** | **List\<AttributeDataTypeCode\>**| 조건 속성 데이터 유형  |
|   **attributeIdPreLike** | **String**| 조건 속성 ID (전방 일치)  |
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |
|   **descriptionLike** | **String**| 조건 속성 설명 (부분 일치)  |
|   **roleIdPreLike** | **String**| 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 역할 ID 목록 (완전 일치)  |















### Response Body

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



#### SearchAttributes.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributes** | **List\<AttributeBundleProtocol\>**| 조건 속성 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |

#### AttributeBundleProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeCreationTypeCode** | **String**|   COMMON_ATTRIBUTE, ROLE_ATTRIBUTE |
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **attributeRoleRelations** | **List\<AttributeRoleRelationProtocol\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTags** | **List\<AttributeTagProtocol\>**| 조건 속성 태그 ID  |
|   **description** | **String**| 조건 속성 설명  |





#### AttributeRoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regYmdt** | **Date**| 조건 속성과 연관된 역할 ID 생성일시  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |












#### AttributeTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeTagId** | **String**| 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| 조건 속성 태그 생성일시  |





























<a name="updateAttribute"></a>
## **조건 속성 수정**
> PUT "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
| Request Body | **UpdateAttribute.Request** | **UpdateAttribute.Request**|  | |




#### UpdateAttribute.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeDataTypeCode** | **String**|   STRING, NUMERIC, DAY_OF_WEEK, DATETIME, TIME, IPADDRESS, BOOLEAN |
|   **attributeName** | **String**| 조건 속성 이름  |
|   **attributeRoleRelationIds** | **List\<String\>**| 조건 속성과 연관된 역할 ID 목록  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록  |
|   **description** | **String**| 조건 속성 설명  |














### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```






# AttributeDataType


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/data-types**](AttributeDataTypeApi.md#getAttributeDataType) | 조건 속성 데이터 타입 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/condition/validate**](AttributeDataTypeApi.md#validateConditionValues) | 조건 값 유효성 체크 |


<a name="getAttributeDataType"></a>
## **조건 속성 데이터 타입 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/data-types"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |







### Response Body

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



#### GetAttributeDataTypeResponse


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **dataTypes** | **List\<GetAttributeDataTypeResponse.AttributeDataTypeProtocol\>**| 조건 속성 데이터 타입 목록  |

#### GetAttributeDataTypeResponse.AttributeDataTypeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **dataType** | **String**| 조건 속성 데이터 타입  |
|   **operators** | **List\<GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol\>**| 조건 속성 사용가능한 연산자 목록  |


#### GetAttributeDataTypeResponse.AttributeOperatorTypeProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **max** | **Integer**| 연산자가 사용할수 있는 값의 최대갯수  |
|   **min** | **Integer**| 연산자가 사용할수 있는 값의 최소갯수  |
|   **operatorTypeCode** | **String**| 연산자  |



























<a name="validateConditionValues"></a>
## **조건 값 유효성 체크**
> POST "/role/v3.0/appkeys/{appKey}/attributes/condition/validate"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **ValidateConditionValuesRequest** | **ValidateConditionValuesRequest**|  | |



#### ValidateConditionValuesRequest


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **conditions** | **List\<ConditionProtocol\>**| 역할 조건 속성  |

#### ConditionProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeOperatorTypeCode** | **String**|   ALL_CONTAINS, ANY_CONTAINS, NOT_CONTAINS, ANY_MATCH, NONE_MATCH, BETWEEN, BEYOND, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO, LESS_THAN, LESS_THAN_OR_EQUAL_TO, ALLOW, NOT_ALLOW, TRUE, FALSE |
|   **attributeValues** | **List\<String\>**| 조건 속성 값  |
















### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```






# AttributeRoleRelation


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](AttributeRoleRelationApi.md#createAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles**](AttributeRoleRelationApi.md#deleteAttributeRoleRelations) | 조건 속성과 연관된 역할 다건 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search**](AttributeRoleRelationApi.md#searchAttributeRoleRelations) | 조건 속성과 연관된 역할 목록 조회 |


<a name="createAttributeRoleRelations"></a>
## **조건 속성과 연관된 역할 다건 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
| Request Body | **CreateAttributeRoleRelations.Request** | **CreateAttributeRoleRelations.Request**|  | |




#### CreateAttributeRoleRelations.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeRoleRelationIds** | **List\<String\>**| 조건 속성과 연관된 역할 ID 목록  |










### Response Body

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
## **조건 속성과 연관된 역할 다건 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
| Request Body | **DeleteAttributeRoleRelations.Request** | **DeleteAttributeRoleRelations.Request**|  | |




#### DeleteAttributeRoleRelations.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeRoleRelationIds** | **List\<String\>**| 조건 속성과 연관된 역할 ID 목록  |










### Response Body

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
## **조건 속성과 연관된 역할 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/roles/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchAttributeRoleRelations.Request** | **SearchAttributeRoleRelations.Request**|  | |




#### SearchAttributeRoleRelations.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **roleIdPreLike** | **String**| 조건 속성과 연관된 역할 ID (전방 일치)  |
|   **roleIds** | **List\<String\>**| 조건 속성과 연관된 역할 ID 목록 (완전 일치)  |
|   **searchRoleOptionCode** | **String**|   DIRECT_ROLE, INDIRECT_ROLE |












### Response Body

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



#### SearchAttributeRoleRelations.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeRoleRelations** | **List\<AttributeRoleRelationProtocol\>**| 조건 속성 연관관계 Role 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |

#### AttributeRoleRelationProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **description** | **String**| 역할 설명  |
|   **exposureOrder** | **Integer**| 노출 순서  |
|   **regYmdt** | **Date**| 조건 속성과 연관된 역할 ID 생성일시  |
|   **roleGroup** | **String**| 역할 그룹  |
|   **roleId** | **String**| 역할 ID  |
|   **roleName** | **String**| 역할 이름  |



























# AttributeTag


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](AttributeTagApi.md#createAttributeTags) | 조건 속성 태그 생성 |
| **DELETE** |[**/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags**](AttributeTagApi.md#deleteAttributeTags) | 조건 속성 태그 삭제 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/id**](AttributeTagApi.md#searchAttributeTagIds) | 조건 속성 태그 ID 목록 조회 |
| **POST** |[**/role/v3.0/appkeys/{appKey}/attributes/tags/search**](AttributeTagApi.md#searchAttributeTags) | 조건 속성 태그 목록 조회 |


<a name="createAttributeTags"></a>
## **조건 속성 태그 생성**
> POST "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
| Request Body | **CreateAttributeTags.Request** | **CreateAttributeTags.Request**|  | |




#### CreateAttributeTags.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록  |










### Response Body

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
## **조건 속성 태그 삭제**
> DELETE "/role/v3.0/appkeys/{appKey}/attributes/{attributeId}/tags"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Path |**attributeId** | **String**| 조건 속성 ID | [default to null] |
| Request Body | **DeleteAttributeTags.Request** | **DeleteAttributeTags.Request**|  | |




#### DeleteAttributeTags.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록  |










### Response Body

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
## **조건 속성 태그 ID 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/id"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchAttributeTagIds.Request** | **SearchAttributeTagIds.Request**|  | |



#### SearchAttributeTagIds.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeIdPreLike** | **String**| 조건 속성 ID (전방 일치)  |
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeTagIdPreLike** | **String**| 조건 속성 태그 ID (전방 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |













### Response Body

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



#### SearchAttributeTagIds.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |


















<a name="searchAttributeTags"></a>
## **조건 속성 태그 목록 조회**
> POST "/role/v3.0/appkeys/{appKey}/attributes/tags/search"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
|  Query |**page** | **Integer**| 검색을 원하는 페이지 번호 (기본값 1) |
|  Query |**itemsPerPage** | **Integer**| 결과를 원하는 페이지별 검색 개수 (기본값 10) |
|  Query |**sort** | **List\<String\>**| 정렬 순서 |  |
| Request Body | **SearchAttributeTags.Request** | **SearchAttributeTags.Request**|  | |



#### SearchAttributeTags.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeIdPreLike** | **String**| 조건 속성 ID (전방 일치)  |
|   **attributeIds** | **List\<String\>**| 조건 속성 ID 목록 (완전 일치)  |
|   **attributeTagIdPreLike** | **String**| 조건 속성 태그 ID (전방 일치)  |
|   **attributeTagIds** | **List\<String\>**| 조건 속성 태그 ID 목록 (완전 일치)  |













### Response Body

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



#### SearchAttributeTags.Response


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeTags** | **List\<AttributeTagProtocol\>**| 조건 속성 태그 목록  |
|   **totalItems** | **Long**| 역할 전체 개수  |

#### AttributeTagProtocol


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **attributeId** | **String**| 조건 속성 ID  |
|   **attributeTagId** | **String**| 조건 속성 태그 ID  |
|   **regYmdt** | **Date**| 조건 속성 태그 생성일시  |























# 설정


| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| **PUT** |[**/role/v3.0/appkeys/{appKey}/config/cache-evict**](TenantConfigurationApi.md#deleteCache) | Role Service 서버와 Client SDK 의 Cache 제거 |
| **GET** |[**/role/v3.0/appkeys/{appKey}/config**](TenantConfigurationApi.md#getConfiguration) | 설정 조회 |
| **PUT** |[**/role/v3.0/appkeys/{appKey}/config**](TenantConfigurationApi.md#updateConfig) | 설정 수정 |


<a name="deleteCache"></a>
## **서버와 Client SDK 의 Cache 제거**
> PUT "/role/v3.0/appkeys/{appKey}/config/cache-evict"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |







### Response Body

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
## **설정 조회**
> GET "/role/v3.0/appkeys/{appKey}/config"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |







### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```



#### GetTenantConfigResponse


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**|   IDENTICAL_PATH, NON_IDENTICAL_PATH |

















<a name="updateConfig"></a>
## **설정 수정**
> PUT "/role/v3.0/appkeys/{appKey}/config"


### Parameters



| | Name | Type | Description  | 
|------------- |------------- | ------------- | ------------- | 
|  Header |**X-Secret-Key** | **String**| SecretKey |  |
|  Path |**appKey** | **String**| 앱키 | [default to null] |
| Request Body | **UpdateConfig.Request** | **UpdateConfig.Request**|  | |



#### UpdateConfig.Request


| Name | Type | Description | 
|------------ | ------------- | ------------- | 
|   **cacheSize** | **Integer**| 리소스 ID 기반 인증 캐시 크기  |
|   **cacheSizeByPath** | **Integer**| 리소스 Hierarchy 조회 캐시 크기  |
|   **cacheSizeTree** | **Integer**| 리소스 Path 기반 인증 캐시 크기  |
|   **cacheTtl** | **Integer**|   |
|   **resourcePathTrailingSlashMatchPolicyCode** | **String**|   IDENTICAL_PATH, NON_IDENTICAL_PATH |














### Response Body

```json
{
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : "resultMessage"
  }
}
```

