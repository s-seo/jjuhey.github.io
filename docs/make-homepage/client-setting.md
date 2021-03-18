---
layout: post
title: 'Client: create-react-app'
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 3
last_modified_date: 2021-03-18 20:40
lastmod: 2021-03-18 20:40
---

# **create-react-app으로 클라이언트쪽 세팅하기**

## create-react-app 설치
1. 터미널을 켜고 원하는 경로로 들어간다.
2. `npx create-react-app my-app` 입력 
    * npx는 npm5.2+ 버전의 패키지 실행 도구
    * my-app: 본인의 어플 이름을 여기에 작성해주면 됨
3. 그러면 그 경로에 `my-app` 이라는 폴더가 생긴다.
    ```
    ├ 📁 node_modules
    ├ package.json
    ├ 📁 public
    ├ README.md
    ├ 📁 src
    ⎣ yarn.lock
    ```
    * package.json과 yarn.lock은 우리가 설치한 패키지에 대한 정보와 초기 설정에 대한 정보를 가지고 있다.
    * README.md는 깃허브 레포에 들어가면 처음 보이는 그 문서이다.
    * src에 가장 중요한 우리 어플리케이션의 index.js, App.js가 있다.
4. `cd my-app` & `yarn start`: 어플리케이션을 실행해본다.
    <img width="500" alt="create-react-app" src="https://user-images.githubusercontent.com/53938072/111625289-080fe800-8830-11eb-81a7-7a95843ff9a0.png">
    * React의 로고가 잘 돌아가고 있으면, 잘 실행되는 것이다.
    * 해당 터미널에서 `ctrl + c`를 누르면 종료된다.

## Remote Repository에 내 어플을 추가
내가 만든 웹 어플리케이션을 지속해서 개발하기 위해, 그리고 내가 어떤 작업내용을 했는지 기록하기 위해서는 원격 저장소에 저장해놓는 것이 좋다.
물론 협업할 때 가장 좋지만, 내가 혼자 개발하더라도 git commit들을 계속 추가하면서 기록을 남겨놓는 것이 좋다.
1. github 홈페이지로 이동한다.
2. Repositories의 우측에 있는 `new`를 눌러준다.
3. 아래와 같이 어플 이름을 입력하고, public으로 만들어준다.
    <img width="400" alt="new repository" src="https://user-images.githubusercontent.com/53938072/111625303-0ba36f00-8830-11eb-8124-a2d538fd6f3a.png">
    * 남들이 내 레포를 보지 않기를 바란다면, private으로 만들어준다.
    * 이번 시리즈는 heroku를 통해 배포까지 진행할 예정이므로 public으로 만들도록 하겠다.
4. 아래와 같이 새로운 레포가 생성되는 것을 볼 수 있다!
    <img width="500" alt="my remote repository" src="https://user-images.githubusercontent.com/53938072/111625312-0e9e5f80-8830-11eb-98ce-95659f98dec6.png">
5. 터미널에 내 my-app의 루트에서(`cd my-app`) 다음과 같이 입력한다.
    ```
    git remote add origin git@github.com:JJuhey/my-app.git
    ```
6. `git push -u origin master`: 원격 레포로 푸시한다.
7. 깃허브 레포로 들어가면 내 로컬에 만들어진 파일들이 그대로 올라간 것을 볼 수 있다!
    <img width="500" alt="uploaded" src="https://user-images.githubusercontent.com/53938072/111625323-11995000-8830-11eb-8485-ba92bfae031a.png">

