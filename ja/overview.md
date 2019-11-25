## Application Service > ROLE > 개요

## Role 상품은?

Role Based Access Control 을 할수 있게 API 및 UI를 제공하는 TOAST Cloud 상품이다.  
Role을 정의하고 각 Resource에 접근 가능한 Role을 설정한 후, User에게 Role을 할당하면, User의 Resource에 대한 접근 권한을 확인할 수 있다.  
Resource를 계층 구조로 저장하기 때문에, Role을 기반으로 접근 가능한 메뉴를 보다 쉽게 구할 수 있다.  

## 전체 구조

![[그림 1] RBAC 시스템](http://static.toastoven.net/prod_role/role_01.png)
<center>[그림 1] RBAC 시스템</center>

![[그림 2] 시스템 구성도](http://static.toastoven.net/prod_role/role_02.png)
<center>[그림 2] 시스템 구성도</center>

## 특장점

* 역할 기반의 접근 제어를 보다 쉽게 사용할 수 있다.  
* 역할 상속 기능을 제공한다.  
* RESTFUL API의 AntPath Pattern 방식의 Resource를 지원한다.  
* 역할에 따른 메뉴를 보다 쉽게 구성 할 수 있다.  

## 주요기능

역할 기반의 권한 체크  
역할에 따른 메뉴 구성  

## 서비스 용어

|용어|	설명|
|---|---|
|User|	권한을 가지는 주체|
|Role|	역할|
|Scope|	역할의 범위|
|Operation|	Resource에 행할 수 있는 행위|
|Resource|	역할이 접근할 수 있는 모든 대상|
