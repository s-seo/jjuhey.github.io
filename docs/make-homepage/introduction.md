---
layout: post
title: Introduction
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 1
last_modified_date: 2021-03-13 13:43
date: 2021-03-13
sitemap:
changefreq: monthly
priority: 0.8
---

## 개요 및 개발 계획

### 목표
Node.js로 개발 환경 셋팅부터 배포까지 모두 경험해본다.
server/client 구축 예정이며, 서버는 [express](https://expressjs.com/ko/)를 사용하고 클라이언트는 [create-react-app](https://ko.reactjs.org/docs/create-a-new-react-app.html)을 이용하여 초기셋팅 할 예정이다.
배포툴은 [Heroku](http://heroku.com)를 이용할 예정이며, 모든 개발환경 및 배포는 일단 초보가 하기에 가장 쉬운 것 기준으로 하였다.

완성작 또한 개인홈페이지 구축을 목표로 하고 있기 때문에, 이를 이용해 포트폴리오로 활용할 수 있다.
더 나아가서 추후에 시간이 된다면, heroku에서 AWS로 옮기는 작업도 해보고 싶다.

### 개발환경
* Language: javascript(nodejs), [typescript](https://www.typescriptlang.org)
* Databse: [MongoDB](https://docs.mongodb.com/manual/introduction/)
* Framework/Library: [express](https://expressjs.com/ko/), [create-react-app](https://ko.reactjs.org/docs/create-a-new-react-app.html), [material-ui](https://material-ui.com)
* Deploy: [Heroku](http://heroku.com)
* IDE: VSCode
* Compouter: Macbook pro

### 개발 기능
(개발완료: **bold**, 포스팅완료: <span class='text-purple-000'>link+purple<span>)
* 개발환경 구축
  * **create-react-app를 이용한 client측 구현**
  * **express를 이용한 server측 구현**
  * **git remote repository 생성 및 Pull Request 관리**
  * **client & server를 연결하기 위한 proxy 설정**
  * **mongoDB 설치 및 config 설정**

* **material-UI 설치 및 스타일 적용**
* **react-router-dom 설치 및 static 페이지 만들기**
* **Heroku 배포 준비**
  * **개정 만들고 git repository와 연결**
  * **배포환경에서 mongoDB를 사용할 수 있도록 준비**

* 로그인 기능 구현
  * 유저 생성(sign up) & 로그인 기능 구현
  * 비밀번호 암호화(Encrypt) 처리
  * 유저 인증처리 및 토큰 처리하기
  * 유저 권한 설정
  * 기타 보안강화 관련 설정

* 관리자 페이지 구축
  * 유저/권한 관리 페이지
  * 

* 페이지 만들기
  * 소개 페이지
  * 포트폴리오
    * 팀프로젝트(it duck, HRU)
    * sementic & 반응형 관련 (FrontEnd)
  * 블로그: github 블로그, 네이버블로그, 유튜브 연결
  * 게시판

### Git Remote Repository
바로가기: [https://github.com/JJuhey/heroku-jjuhey](https://github.com/JJuhey/heroku-jjuhey)

### 마무리
이미 개발환경 구축과 배포 테스트까지 완료했다. mongoDB를 배포환경에서 사용 설정하는게 꽤 오래걸렸지만.. 성공했으니, 이제 기능개발에 집중할 수 있는 환경이 다 갖춰졌다!

다만, 여태까지 한 것들을 좀더 잘 정리해서 블로그에 기록해두기 위해 github블로그도 이렇게 지난주에 만들었다!!
기능 개발은 일단 잠시 미뤄두고 여태까지 작업한 개발환경 및 배포환경 셋팅을 어떻게 할지 차근히 포스팅 할 생각이다.
