---
title: '좋은 API URI 설계하기'
date: 2022-02-01
tags:
  - http
  - api
---

## 개요

요즘은 웹 애플리케이션 뿐만 아니라 안드로이드나, iOS 애플리케이션 까지 모두 HTTP API 을 이용하여 데이터를 통신합니다. 이렇게 많이 쓰이는 API의 URI를 보다 잘 설계할수 있는 방법을 알아 보겠습니다.

## HTTP API

### 회원 정보 관리 API

- 회원 목록 조회
  - /read-member-list
- 회원 조회
  - /read-member-by-id
- 회원 등록
  - /create-member
- 회원 수정
  - /update-member
- 회원 삭제
  - /delete-member

회원 정보 관리에 대한 API URI를 작성해 보았습니다. 첫 번째에 해당 항목의 작업에 대한 설명을 하고 그 후에 어떠한 작업을 대한 것을 명시하였습니다.

**하지만 이것이 좋은 URI 설계 일까?**

URI(Uniform Resource Identifier)의 R은 리소스를 의미합니다. 그렇다면 리소스란 무엇일까요? 리소스란 조회, 등록, 수정, 삭제 같은 행위가 아닌 회원이란 자체를 의미합니다. 그렇다면 URI에서는 해당 행위들은 모두 배제하여 회원이란 리소스만 식별하여야 합니다.

### 회원 정보 관리 API

- 회원 목록 조회
  - /members
- 회원 조회
  - /members/{id}
- 회원 등록
  - /members/{id}
- 회원 수정
  - /members/{id}
- 회원 삭제
  - /members/{id}

회원이라는 리소스만으로 식별하여 API 작성한다면 다음과 같이 작성이 될 것입니다. 하지만 조회, 등록, 수정, 삭제에 대한 행위들을 어떻게 구별을 할까요?

## HTTP 메서드

- GET
  - 리소스 조회
  - 서버에 전달하고 싶은 데이터는 query를 통해서 전달
  - 최신 스펙에서는 body를 통해 데이터를 전달할 수 있지만, 지원하지 않는곳이 많아 권장하지 않음
  - 조회하는 메서드에 사용
- POST
  - 요청 데이터 처리
  - 서버에 전달하고 싶은 데이터는 body를 통해서 전달
  - 주로 등록하는 메서드에 사용하지만 다른 메서드로 처리하기 애매한 경우 POST로 처리함
- PUT
  - 리소스 대체
  - 서버에 전달하고 싶은 데이터는 query와 body를 통해서 전달
  - 리로스가 있다면 대체하고 없다면 생성(Copy & Paste)
  - 수정하는 메서드에서 사용되지만 모든 리소스를 대체하기 때문에 사용처가 한정적임
- PATCH
  - 리소스 부분 변경
  - 서버에 전달하고 싶은 데이터는 query와 body를 통해서 전달
  - PUT과는 다르게 body를 통하여 전달한 데이터만 부분적으로 변경
  - 수정하는 메서드에 사용
- DELETE
  - 리소스 삭제
  - 서버에 전달하고 싶은 데이터는 query를 통해서 전달
  - 삭제하는 메서드에 사용

## HTTP 메서드를 이용한 API 설계

### 회원 정보 관리 API

- 회원 목록 조회
  - GET /members
- 회원 조회
  - GET /members/{id}
- 회원 등록
  - POST /members/{id}
- 회원 수정
  - PATCH /members/{id}
- 회원 삭제
  - DELETE /members/{id}

## 참고

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
