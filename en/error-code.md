## Application Service > ROLE > 오류 코드
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
|--|--|--|
| TRUE | 0 | 성공 |
| FALSE | -2 | 서버 문제로 인한 실패. 담당자에게 확인 필요 |
| FALSE | 400 | 잘못된 인자로 인한 API 호출 실패 |
| FALSE | 401 | 유효하지 않은 appkey, secretkey |
| FALSE | 403 | 허락되지 않은 IP에서 호출이 되어 실패 |
| FALSE | 404 | API URL 및 HTTP 메서드를 확인하세요 |
| FALSE | 500 | 서버 문제로 인한 실패. 담당자에게 확인 필요 |
| FALSE | 1000 | 유효하지 않은 요청 매개변수 |
| FALSE | 1002 | 유효성 검증이 실패한 요청 매개변수 |
| FALSE | 1003 | 서버 문제로 응답 반환 실패 |
| FALSE | 1004 | 서버 문제로 인한 실패 |
| FALSE | 1005 | 서버 문제로 인한 실패 |
| FALSE | 1200 | 서버 문제로 인한 실패 |
| FALSE | 1201 | 서버 문제로 인한 실패 |
| FALSE | 4003 | 유효하지 않은 appkey |
| FALSE | 10000 | 유효하지 않은 리소스 Path |
| FALSE | 10001 | 유효하지 않은 리소스 이름 |
| FALSE | 10002 | 유효하지 않은 리소스 ID |
| FALSE | 10003 | 중복된 리소스 Path 가 존재 |
| FALSE | 10004 | 중복된 리소스가 존재 |
| FALSE | 10005 | 리소스를 찾을 수 없음 |
| FALSE | 10006 | 너무 많은 리소스가 등록되어 있음 |
| FALSE | 10007 | 중복된 리소스 UI Path |
| FALSE | 10008 | 유효하지 않은 리소스 UI Path 이름 |
| FALSE | 11000 | 중복된 역할이 존재 |
| FALSE | 11001 | 역할을 찾을 수 없음 |
| FALSE | 11002 | 유효하지 않은 역할 ID |
| FALSE | 11003 | 역할 관계가 이미 존재 |
| FALSE | 11004 | 역할 관계를 찾을 수 없음 |
| FALSE | 11005 | 역할과 리소스 관계가 이미 존재 |
| FALSE | 11007 | 역할 관계 지정할 수 없는 역할이 지정됨 |
| FALSE | 11008 | 역할 관계 설정 매개변수 오류 |
| FALSE | 11010 | 거부로 설정할 수 없음 |
| FALSE | 12000 | 오퍼레이션을 찾을 수 없음 |
| FALSE | 12001 | 오퍼레이션이 이미 존재 |
| FALSE | 12002 | 유효하지 않은 오퍼레이션 ID |
| FALSE | 13000 | 범위 ID가 이미 존재 |
| FALSE | 13001 | 범위 ID를 찾을 수 없음 |
| FALSE | 13002 | ALL 범위에 대한 변경은 허용하지 않음 |
| FALSE | 13003 | 유효하지 않은 범위 ID |
| FALSE | 14000 | 사용자 ID가 이미 존재 |
| FALSE | 14001 | 사용자 ID를 찾을 수 없음 |
| FALSE | 14002 | 사용자 역할 관계가 이미 존재 함 |
| FALSE | 14003 | 사용자 역할 관계를 찾을 수 없음 |
| FALSE | 14004 | 유효하지 않은 사용자 ID |
| FALSE | 14005 | 유효하지 않은 기간 설정 |
| FALSE | 15000 | Excel 파일 처리 실패 |
| FALSE | 15001 | 잘못된 Excel 파일 형식 |
| FALSE | 20000 | 이미 존재하는 조건 속성 ID |
| FALSE | 20001 | 유효하지 않은 조건 속성 ID |
| FALSE | 20005 | 이미 사용 중인 조건 속성 ID |
| FALSE | 21001 | 조건 속성값의 연산자 유형이 허용되지 않음 |
| FALSE | 21002 | 조건 속성값의 데이터 유형이 잘못됨 |
| FALSE | 90000 | 시스템적인 문제로 실패. 담당자에게 확인 필요 |
