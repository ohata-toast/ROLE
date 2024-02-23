## Application Service > ROLE > 콘솔 사용 가이드

## 게시판 예제

작은 게시판을 만드는 상황에서 역할 기반 리소스 접근 제어를 구성하는 예제로 콘솔 사용법을 설명하겠습니다.
`/board/v1.0/{boardId}` API를 호출하면 게시물을 반환하는 API가 있고, 이 API는 인증된 회원만 호출할 수 있다고 가정하겠습니다.
먼저 인증된 회원이라는 역할을 만들어야 합니다.

> curl을 사용한 예제에서 "\{Appkey}" 와 "\{SecretKey}" 값은 실제 프로젝트 내의 활성화한 ROLE 서비스의 앱키와 비밀 키로 대체해야 합니다.

### 1) 역할 생성

![role_1.1.png](http://static.toastoven.net/prod_role/role_1.1.png)
<center>[그림 1.1] 역할 탭으로 이동합니다.</center>

![role_1.2.png](http://static.toastoven.net/prod_role/role_1.2.png)
<center>[그림 1.2] 역할을 추가합니다.</center>

### 2) 오퍼레이션 생성

역할을 만들었으면 오퍼레이션을 만들어야 합니다.

![role_2.1.png](http://static.toastoven.net/prod_role/role_2.1.png)
<center>[그림 2.1] 오퍼레이션 탭으로 이동합니다.</center>

![role_2.2.png](http://static.toastoven.net/prod_role/role_2.2.png)
<center>[그림 2.2] 오퍼레이션을 추가합니다.</center>

### 3) 리소스 생성

이제 `/board/v1.0/{boardId}`를 리소스로 등록해 보겠습니다.
`board`, `v1.0`, `{boardId}`로 나누어서 순차적으로 등록해야 합니다.

![role_3.1.png](http://static.toastoven.net/prod_role/role_3.1.png)
<center>[그림 3.1] 리소스 탭으로 이동합니다.</center>

![role_3.2.png](http://static.toastoven.net/prod_role/role_3.2.png)
<center>[그림 3.2] 리소스를 추가하고자 하는 부모 노드를 클릭하고, 상단에 <strong>추가</strong> 버튼을 클릭합니다.</center>

![role_3.3.png](http://static.toastoven.net/prod_role/role_3.3.png)
<center>[그림 3.3] 리소스 #1 `board`를 추가합니다.</center>

![role_3.4.png](http://static.toastoven.net/prod_role/role_3.4.png)
<center>[그림 3.4] 리소스 #2 `v1.0`를 추가합니다.</center>

![role_3.5.png](http://static.toastoven.net/prod_role/role_3.5.png)
<center>[그림 3.5] 리소스 #3 `{boardId}`를 추가합니다.</center>

### 4) 역할-리소스 관계 생성

리소스까지 등록했다면 역할이 오퍼레이션을 수행할 수 있는 리소스를 지정하기 위해, `역할-리소스` 관계를 설정해야 합니다.

![role_4.1.png](http://static.toastoven.net/prod_role/role_4.1.png)
<center>[그림 4.1] 사용자-리소스 관계를 추가합니다.</center>

![role_4.2.png](http://static.toastoven.net/prod_role/role_4.2.png)
<center>[그림 4.2] 사용자-리소스 관계 추가 후 모습입니다.</center>

### 5) 조건 속성 생성

생성한 역할에 특정 조건에만 오퍼레이션 수행 권한을 부여하기 위해, `역할-조건 속성` 관계를 설정해야 합니다.
조건 속성은 조건 속성에 미리 추가한 역할에서만 사용할 수 있습니다. 조건 속성을 생성/수정할 때, 앞서 생성해 둔 역할을 조건 속성에 추가해 줍니다.

![role_5.1.png](http://static.toastoven.net/prod_role/role_5.1.png)
<center>[그림 5.1] 조건 속성 탭으로 이동합니다.</center>

![role_5.2.png](http://static.toastoven.net/prod_role/role_5.2.png)
<center>[그림 5.2] 조건 속성 추가 화면입니다.</center>

![role_5.3.png](http://static.toastoven.net/prod_role/role_5.3.png)
<center>[그림 5.3] 조건 속성에 역할을 추가합니다.</center>

### 6) 사용자 생성

마지막으로 게시판 API를 사용할 사용자를 추가하고, 접근 제어를 설정하기 위해 `MEMBER` 역할과 `instance.name`조건 속성을 설정합니다.

![role_6.1.png](http://static.toastoven.net/prod_role/role_6.1.png)
<center>[그림 6.1] 사용자 탭으로 이동합니다.</center>

![role_6.2.png](http://static.toastoven.net/prod_role/role_6.2.png)
<center>[그림 6.2] 사용자 추가 화면입니다.</center>

![role_6.3.png](http://static.toastoven.net/prod_role/role_6.3.png)
<center>[그림 6.3] 역할을 추가한 모습입니다.</center>

![role_6.4.png](http://static.toastoven.net/prod_role/role_6.4.png)
<center>[그림 6.4] 추가한 역할 수정 모달입니다.</center>

![role_6.5.png](http://static.toastoven.net/prod_role/role_6.5.png)
<center>[그림 6.5] 역할 수정 모달에서 조건 속성을 추가합니다.</center>

![role_6.6.png](http://static.toastoven.net/prod_role/role_6.6.png)
<center>[그림 6.6] 조건 속성을 추가한 모습입니다.</center>

![role_6.7.png](http://static.toastoven.net/prod_role/role_6.7.png)
<center>[그림 6.7] 사용자의 역할에 조건 속성까지 추가된 모습입니다.</center>

### 7) 권한 체크

`userId`가 Header 의 `'uuid'`로 값이 넘어온다고 가정해 보겠습니다.
`12345678-1234-5678-1234-567812345678` 사용자가 `/board/v1.0/1` API를 호출하였을 때, 권한을 체크하면 아래와 같습니다.

#### [RESTful API 호출 시]

```shell
curl -X POST -H "Content-Type: application/json" -d '{
    "resources": [
        {
            "attributes": {
                "attributeId": "instance.name",
                "attributeValue": "GPU"
            },
            "authRequestId": "",
            "operationId": "GET",
            "resourceId": "",
            "resourcePath": "/board/v1.0/1",
            "scopeId": "ALL"
        }
    ]
}' "https://role.api.nhncloudservice.com/role/v3.0/appkeys/{Appkey}/users/12345678-1234-5678-1234-567812345678/authorizations/resources"
```

응답 예시) 접근 권한이 있는 경우 해당 리소스 내부에 `permission: true`로 응답이 내려옵니다.

```json
{
  "authorizations" : [
    {
      "attributes" : [
        {
          "attributeId" : "instance.name",
          "attributeValue" : "GPU"
        }
      ],
      "operationId" : "GET",
      "permission" : true,
      "resourceId" : "{boardId}",
      "resourcePath" : "/board/v1.0/1",
      "scopeId" : "ALL"
    }
  ],
  "header" : {
    "isSuccessful" : true,
    "resultCode" : 0,
    "resultMessage" : ""
  }
}
```

## 마이그레이션

ROLE 서비스를 사용하는 다른 프로젝트가 있다면, 데이터 이관 기능을 이용해서 편리하게 데이터를 동기화 시킬 수 있습니다.
데이터 동기화의 대상은 `리소스`, `역할`, `오퍼레이션`이며 `범위`와 `사용자`는 동기화되지 않습니다.

관리 탭의 마이그레이션 메뉴 영역에서 데이터를 이관할 프로젝트 혹은 앱키를 입력해서 진행할 수 있습니다.

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[그림 8.1] 관리 탭으로 이동합니다.</center>

![role_8.2.png](http://static.toastoven.net/prod_role/role_8.2.png)
<center>[그림 8.2] 마이그레이션 메뉴입니다.</center>

데이터를 이관할 프로젝트를 선택하거나, 직접 앱키를 입력할 수 있습니다.

![role_8.3.png](http://static.toastoven.net/prod_role/role_8.3.png)
<center>[그림 8.3] 프로젝트 선택 드롭다운의 모습입니다.</center>

![role_8.4.png](http://static.toastoven.net/prod_role/role_8.4.png)
<center>[그림 8.4] 앱키를 입력하는 모습입니다.</center>

![role_8.5.png](http://static.toastoven.net/prod_role/role_8.5.png)
<center>[그림 8.5] <strong>확인</strong> 버튼 클릭 시, 노출되는 확인 모달입니다.</center>

## 서버 설정

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[그림 9.1] 관리 탭으로 이동합니다.</center>

![role_9.2.png](http://static.toastoven.net/prod_role/role_9.2.png)
<center>[그림 9.2] 서버 설정 영역입니다.</center>

### 클라이언트 SDK 캐시 설정

클라이언트 SDK 캐시 설정을 할 수 있습니다.
설정할 수 있는 속성은 `TTL`, `ID별 사이즈`, `Path별 사이즈`, `Tree별 사이즈`가 있습니다.

### Resource Path Trailing Slash Match

리소스 경로의 마지막 `'/'`에 대해 설정할 수 있습니다.
`Non Identical Path`로 설정한다면, `'/board/v1.0/{boardId}'` 와 `'/board/v1.0/{boardId}/'`는 서로 다른 경로입니다.
하지만, `Identical Path`로 설정한다면, `'/board/v1.0/{boardId}'` 와 `'/board/v1.0/{boardId}/'`는 같은 경로입니다.

## 캐시 삭제

클라이언트 SDK 와 서버의 캐시 때문에 변경된 리소스에 대한 권한 체크 결과가 즉시 반영되지 않을 수 있습니다.
그럴 경우 관리 탭의 **캐시 삭제** 버튼을 사용해서 명시적으로 캐시를 삭제하면 문제를 해결할 수 있습니다.

![role_8.1.png](http://static.toastoven.net/prod_role/role_8.1.png)
<center>[그림 10.1] 관리 탭으로 이동합니다.</center>

![role_10.2.png](http://static.toastoven.net/prod_role/role_10.2.png)
<center>[그림 10.2] 캐시 삭제 버튼입니다.</center>

![role_10.3.png](http://static.toastoven.net/prod_role/role_10.3.png)
<center>[그림 10.3] 버튼을 클릭한 후 열리는 삭제 확인 모달입니다.</center>
