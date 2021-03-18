---
layout: post
title: 'Client: create-react-app'
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 3
last_modified_date: 2021-03-18 20:40
lastmod: 2021-03-18 20:40
---

# **create-react-app으로 클라이언트 기본세팅**

## create-react-app 설치
1. 터미널을 켜고 원하는 경로로 들어간다.
<br/><br/>

2. `npx create-react-app my-app`
    * npx는 npm5.2+ 버전의 패키지 실행 도구
    * my-app: 본인의 어플 이름을 여기에 작성해주면 됨
<br/><br/>

3. 그러면 그 경로에 `my-app` 이라는 폴더가 생긴다.
    ```
    ├ 📁 node_modules
    ├ package.json
    ├ 📁 public
    ├ README.md
    ├ .gitignore
    ├ 📁 src
    ⎣ yarn.lock
    ```
    * package.json과 yarn.lock은 우리가 설치한 패키지에 대한 정보와 초기 설정에 대한 정보를 가지고 있다.
    * README.md는 깃허브 레포에 들어가면 처음 보이는 그 문서이다. ([마크다운](https://gist.github.com/ihoneymon/652be052a0727ad59601)에 문법을 쓴다.)
    * src에 가장 중요한 우리 어플리케이션의 index.js, App.js가 있다.
<br/><br/>

4. `cd my-app` & `yarn start`: 어플리케이션을 실행
    <img width="500" alt="create-react-app" src="https://user-images.githubusercontent.com/53938072/111625289-080fe800-8830-11eb-81a7-7a95843ff9a0.png">
    * React의 로고가 잘 돌아가고 있으면, 잘 실행되는 것이다.
    * 해당 터미널에서 `ctrl + c`를 누르면 종료된다.

## Remote Repository 만들기
내가 만든 웹 어플리케이션을 지속해서 개발하기 위해, 그리고 내가 어떤 작업내용을 했는지 기록하기 위해서는 원격 저장소에 저장해놓는 것이 좋다.
물론 협업할 때 가장 좋지만, 내가 혼자 개발하더라도 git commit들을 계속 추가하면서 기록을 남겨놓는 것이 좋다.
1. github 홈페이지로 이동한다.
2. Repositories의 우측에 있는 `new`를 눌러준다.
3. 아래와 같이 어플 이름을 입력하고, public으로 만들어준다.
    <img width="700" alt="스크린샷 2021-03-18 오후 9 06 13" src="https://user-images.githubusercontent.com/53938072/111626802-c122f200-8831-11eb-9010-75ff549b9c89.png">
    * 남들이 내 레포를 보지 않기를 바란다면, private으로 만들어준다.
    * 이번 시리즈는 heroku를 통해 배포까지 진행할 예정이므로 public으로 만들도록 하겠다.
<br/><br/>
4. 아래와 같이 새로운 레포가 생성되는 것을 볼 수 있다.
    <img width="700" alt="my remote repository" src="https://user-images.githubusercontent.com/53938072/111625312-0e9e5f80-8830-11eb-98ce-95659f98dec6.png">
<br/><br/>
5. 터미널에 내 my-app의 루트에서(`cd my-app`) 다음과 같이 입력한다. (레포 이름(my-app), 계정이름(JJuhey)는 본인의 것으로 바꿔야함)
    ```
    git remote add origin git@github.com:JJuhey/my-app.git
    ```
6. `git push -u origin master`
<br/><br/>
7. 깃허브 레포로 들어가면 내 로컬에 만들어진 파일들이 그대로 올라간 것을 볼 수 있다!
    <img width="700" alt="uploaded" src="https://user-images.githubusercontent.com/53938072/111625323-11995000-8830-11eb-8485-ba92bfae031a.png">

## UI 변경해보기
원격 저장소까지 만들어봤으니, 이제 create-react-app에서 자동으로 생성해준 UI를 삭제하고 나만의 UI(?)를 만들어보자.
1. 터미널에서 `code .`를 입력해서 VSCode에서 내 폴더가 열리도록 한다.
    (이때, 안열린다면 vscode에서 `shift+command+p`를 눌러서 Shell Command를 Install한다.)
2. `src/App.js` 파일을 열어서, 다음과 같이 바꿔준다.
    ```javascript
    function App() {
      return (
        <div className="App">
          Hello JJuhey World!
        </div>
      );
    }

    export default App;
    ```
3. 그리고 실행(`yarn start`)해보면, 다음과 같이 바뀌는 것을 알 수 있다.
<img width="700" alt="스크린샷 2021-03-18 오후 11 26 36" src="https://user-images.githubusercontent.com/53938072/111642473-77420800-8841-11eb-93af-7b6f7f494ae9.png">
<br/><br/>

4. 이제 변경사항을 저장해보자.
    ```
    > git add .
    > git commit -m "Change App file"
    ```
    * 각 한줄씩 입력해서 엔터쳐준다. 그러면 각각 unstage => stage => commit 으로 상태가 바뀐다.
    * 각 줄씩 상태확인을 하고 싶다면, 입력한 뒤에 `git status`를 입력해서 상태(변화)를 볼 수 있다.
5. 원격 저장소(Remote Repository)로 반영하기 위해 push한다.
    ```
    > git push origin master
    ```
    * 사실 브랜치를 생성해서 거기에 푸시 & 머지를 해야하는데, 그건 다음 시간에 배우도록 하자.
    * 완료가 되었으면, 깃허브에서 잘 올라갔는지 확인

## 다음 이야기
create-react-app로 이렇게 간단하게 어플을 만들 수 있다. 다음번에는 서버를 만들어보도록 하겠다.
