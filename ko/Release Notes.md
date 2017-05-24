## Upcoming Products > ROLE > Release Notes

### 2017.05.25
#### 버그 수정
* [Console] Resource 탭의 [Excel 업로드] 기능이 동작하지 않는 문제 수정 

### 2017.04.20
#### 버그 수정
* userId(key)의 value 값에 '.', '@' 포함될 경우 오류가 반환되는 버그 수정

### 2016.12.22
#### 기능 개선/변경
* 벌크 User 리스트 조회 API 추가
* Scope과 연관된 연관 관계 조회 API 추가

#### 버그 수정
* Role 연관 관계가 제대로 동작하지 않던 문제 수정
* User 검색 시, ScopeID ALL 이 검색되지 않는 문제 수정
* invalid 한 resource tree 가 있을 시, hierarchy tree 가 비정상적으로 반환되는 버그 수정

### 2016.11.24
#### 기능 개선/변경
* Client SDK 및 서버의 Cache 제거하는 기능 추가

#### 버그 수정
* Resource Path 수정 시 '/' 로 시작하지 않는 잘못된 Path 로 수정 할 수 없도록 수정
	* 잘못된 Path 가 있을 시 Excel 을 이용한 데이터 입력이 안될 수 있음

### 2016.10.20
#### 기능 개선/변경
* User 목록 조회시, 연관 관계에 있는 Role 을 가진 사용자도 반환 하는 옵션 추가
	* 자세한 사항은 메뉴얼 참고(http://docs.cloud.toast.com/ko/Upcoming%20Products/ROLE/RESTFUL%20API%20가이드/#1-user)

### 2016.09.29
#### 기능 개선/변경
* User 에 신규 Role 부여시, 기존에 등록된 Role 중 Scope 이 같은 Role 을 삭제하는 API 추가
* Role 에 User 추가 API 에서 User 가 없으면 User 를 생성 해주는 옵션 추가
	* 자세한 사항은 메뉴얼 참고(http://docs.cloud.toast.com/ko/Upcoming%20Products/ROLE/RESTFUL%20API%20가이드/#3-role)

### 2016.08.18
#### 기능 개선/변경
* 떨어지는 사용성으로 인하여, Polling API 지원 deprecate 됨
* Role 상품을 이용하는 Project 간 데이터를 Migration 하는 기능 추가
    * 자세한 사항은 메뉴얼 참고(http://docs.cloud.toast.com/ko/Upcoming%20Products/ROLE/Getting%20Started/#_3)

#### 버그 수정
* Role을 삭제했는데 다른 Role의 연관 정보가 삭제되는 버그 수정
* 권한체크가 간헐적으로 실패하는 버그 수정
