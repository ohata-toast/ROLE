## Upcoming Products > ROLE > Release Notes

### 2016.09.29
#### 기능 개선/변경
* User 의 모든 Role 을 삭제 후, 신규 Role 을 부여하는 API 추가
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