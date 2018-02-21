## Application Service > ROLE > Error Code
Response Body에는 "header" 정보가 기본으로 포함되어 있습니다.
API 호출이 실패하면 isSuccessful이 false가 되며, 오류 코드가 resultCode에 표시됩니다.

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```
resultCode는 아래와 같이 정의되어 있습니다.

| isSuccessful | resultCode | 설명 |
|--- |--- |--- |
| true | 0 | 성공 |
| false | 400 | 잘못된 인자로 인한 API 호출 실패 |
| false | 401 | 유효하지 않은 appkey, secretkey |
| false | 403 | 허락되지 않은 IP에서 호출이 되어 실패 |
| false | 500 | 서버 문제로 인한 실패. 담당자에게 확인 필요 |
| false | 10000 | 유효하지 않은 resource path |
| false | 10001 | 유효하지 않은 resource 이름 |
| false | 10002 | 유효하지 않은 resource id |
| false | 10003 | 중복된 resource path 가 존재 |
| false | 10004 | 중복된 resource 가 존재 |
| false | 10005 | resource 를 찾을 수 없음 |
| false | 10006 | 32767 개 이상의 리소스를 등록할 수 없어서 실패 |
| false | 10007 | 중복된 resource ui path |
| false | 10008 | 유효하지 않은 resource ui path 이름|
| false | 11000 | 중복된 role 이 존재 |
| false | 11001 | role 을 찾을 수 없음 |
| false | 11002 | 유효하지 않은 role id |
| false | 11003 | role 관계가 이미 존재 |
| false | 11004 | role 관계를 찾을 수 없음 |
| false | 11005 | role resource 관계가 이미 존재 |
| false | 12000 | operation 을 찾을 수 없음 |
| false | 12001 | operation 이 이미 존재 |
| false | 12002 | 유효하지 않은 operation id |
| false | 13000 | scope 이 이미 존재 |
| false | 13001 | scope 을 찾을 수 없음 |
| false | 13002 | ALL scope 에 대한 변경은 허용하지 않음 |
| false | 13003 | 유효하지 않은 scope id |
| false | 14000 | user 가 이미 존재 |
| false | 14001 | user 를 찾을 수 없음 |
| false | 14002 | user role 관계가 이미 존재 함 |
| false | 14003 | user role 관계를 찾을 수 없음 |
| false | 14004 | 유효하지 않은 user id |
| false | 15000 | 엑셀 작업에 실패함 |
| false | 15001 | 유효하지 않은 엑셀 형식 |
| false | 90000 | 시스템적인 문제로 실패. 담당자에게 확인 필요|

