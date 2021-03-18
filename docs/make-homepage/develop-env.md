---
layout: post
title: 개발환경 구축하기
parent: Node.js 웹앱 구축 & Heroku 배포
nav_order: 2
last_modified_date: 2021-03-17 23:40
lastmod: 2021-03-17 23:40
---

# **개발환경 구축하기**

### Homebrew 설치
Homebrew는 macOS용 패키지 관리자이다. mvn, node, yarn 등을 설치할 수 있게 도와준다.

* [Homebrew 홈페이지](https://brew.sh/index_ko)를 들어가서 설치하기 밑에 있는 명령어를 복사한다.
* terminal을 켜서 다음과 같은 명령어를 입력한다. (홈페이지서 복사한 것과 동일하다.)
  ```
  $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

### nvm 설치
Node Version Manage: 노드 버전을 관리해준다. node, npm 등을 관리해주며, nodejs를 사용하는 개발자는 필수로 설치한다.
* homebrew로 설치한다.
  ```
  $ brew install nvm
  ```
* 설치가 완료되었으면, 터미널에서 환경변수를 변경하라는 메시지가 나온다. 환경변수 설정을 해보자.
  ```
  $vim ~/.zshrc
  ```
* Insert(`i`)모드로 전환해서 해당 내용을 붙여준다.
  ```
  export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"
  [ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"
  ```
* 다시 보기모드(esc)로 전환해서 저장 후 종료(`:wq`)한다.
* 재시작을 해준다.
  ```
  source ~/.zshrc
  ```
* `nvm ls`를 입력했을 때, 정보가 잘 나오면 일단 성공.
* Node.js를 설치한다.
  ```
  nvm install 10
  ```
  (여기서 10은 버전을 뜻한다. 보통은 안정된 버전 혹은 회사에서 사용하는 버전이 따로 있으니 이렇게 버전을 지정해서 설치하는 것이 좋다.)
* `nvm ls`를 입력했을 때, 10버전이 잘 설치되었는지 확인해본다.

### 기타 툴 설치
1. 터미널 설치
* kitty: `brew install --cask kitty`
* iterm: [https://iterm2.com/](https://iterm2.com/)

2. VSCode 설치: [https://code.visualstudio.com/](https://code.visualstudio.com/)
* 유용한 extension: `Babel JavaScript`, `Beautify`, `Bracket Pair Colorizer2`, `bracket-padder`, `colorize`, `CSS Modules`, `CSScomb`, `Debugger for Chrome`, `ESLint`, `file-icons`, `Git History`, `gitignore`, `GitLens`, `IntelliSense for CSS class`, `JS JSX Snippets`, `Prettier ESLint`, `Sass`, `TODO Highlight`, `TSLint`

3. git 설치
`brew install git`