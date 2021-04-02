---
layout: post
title: 'Server: Express.js로 구축'
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 4
---

# **Express.js로 서버측 세팅하기**
(아직 작성중....)

## Client측을 분리하기
현재 디렉토리 구조는 다음과 같다.
```shell
├ 📁 public
├ 📁 src
├ .gitignore
├ README.md
├ package.json
⎿ yarn.lock
```

현재 `create-react-app`을 이용해서 프론트엔드를 구축했었고 이를 백엔드와 구분하기 위해서 루트경로에 client 폴더를 생성하고 그 폴더에 기존에 루트경로에 있던 폴더/파일을 옮기도록 하겠다. 그래서 이렇게 바뀌게 된다.
```shell
├ 📁 server
⎿ 📁 client
    ├ 📁 public
    ├ 📁 src
    ├ README.md
    ├ package.json
    ⎿ yarn.lock
├ .gitignore
├ package.json
⎿ yarn.lock
```
* 이렇게 

## Server 폴더 생성 & Express로 백엔드 구축하기
