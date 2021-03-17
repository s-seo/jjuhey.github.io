---
layout: post
title: mongoDB(atlas) 연결하기
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 3
---

## MongoDB 연결하기
> 목표: mongoDB Atlas서비스를 이용해서 개발 및 배포환경에서 DB 연결하기


### Atlas 계정 만들기 & Collection 만들기
* [MongoDB atlas](https://www.mongodb.com/) 사이트에 들어간다.

### 개발환경에서 mongoDB 설정 & 패키지 설치
* mongoose 설치
* process.env.NODE_ENV를 이용해서 DB주소 입력
* `.gitignore`에 `dev.js` 파일추가
* Schema 작성 및 개발환경에서 테스트

### Production 환경에서 사용할 수 있도록 Heroku setting
* 카드 등록
* Add-on 설치
* setting에서 terminal생성
* 배포하기
* ip address를 atlas에 등록
* 