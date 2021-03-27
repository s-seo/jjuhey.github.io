---
layout: post
title: Babel & Typescript 적용하기
parent: 오답노트(trouble shooting)
nav_order: 3
last_modified_date: 2021-03-27 15:20
lastmod: 2021-03-27 15:20
---

## Typescript & Import/Export 사용하기

### **Problem**
* 요즘 Heroku로 개인홈페이지 구축하는 사이드 프로젝트 진행중이다. [참고](https://jjuhey.github.io/docs/make-homepage/)
* 프론트엔드는 `create-react-app`으로 간단하게 만들 수 있었는데, 백엔드 같은 경우는 내가 다 하나하나 설정해줘야 하니 어지간히 힘든게 아니다. (갑분 찡찡;;)
* 암튼 지금은 `javascript`로 적용된 코드들을 정확한 타입을 사용해서 디버깅이 쉽게 되도록 `typescript`로 바꾸고 싶었다.
    ```
    index.js -> index.ts
    ```
* 더불어 지금 import/export 키워드를 못쓰고 계속 require로 쓰고있는데, 이것도 바꾸고 싶었다.
    ```javascript
    const express = require('express') // 기존에 이렇게 쓰여있던 걸
    import express from 'express' // 이렇게 바꾸고 싶다!
    ```
* typescript까지는 잘 적용되었는데, 자꾸 Unexpected identifier가 일어났다. ([STEP 3](https://jjuhey.github.io/docs/trouble-shooting/typescript-babel/#step-3--%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%88%98%EC%A0%95)로 해결했다.)
    ```shell
    import express from 'express';
           ^^^^^^^
    SyntaxError: Unexpected identifier
        at Module._compile (internal/modules/cjs/loader.js:723:23)
        at Module._extensions..js (internal/modules/cjs/loader.js:789:10)
    ```

* * *

### **Solution**
* 해야할 일은 2가지이다.
  1. typescript 적용
  2. babel preset 설정
* 그리고 이 두개를 이어주는 작업(typescript-preset)도 해줘야 한다. (이부분에서 계속 막혔었다ㅠㅠ)
* 현재 의존성 설치된 목록
    ```json
    "dependencies": {
      "bcrypt": "^5.0.1",
      "body-parser": "^1.19.0",
      "concurrently": "^6.0.0",
      "cookie-parser": "^1.4.5",
      "express": "^4.17.1",
      "jsonwebtoken": "^8.5.1",
      "mongoose": "5.11.15"
    },
    "devDependencies": {
      "@babel/cli": "^7.13.10",
      "@babel/core": "^7.13.13",
      "nodemon": "^2.0.7",
    }
    ```

### STEP 1 | Typescript 적용
* typescript관련 의존성 설치 (express는 되는지 확인하기 위해 같이 설치해줌)
    ```shell
    > yarn add -D typescript
    > yarn add -D @types/express
    ```
* tsconfig.json 설정 (근데 이건..너무 인터넷을 많이 찾아봐서 여러 사이트것이 섞여있다. 나중에 한번 체크하고 필요없는 것을 지워주고 고칠것은 고쳐야 할듯하다.)
    ```json
    {
      "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "sourceMap": true,
        "outDir": "dist",
        "strict": true,
        "moduleResolution": "node",
        "esModuleInterop": true,
      },
      "exclude": [
        "node_modules"
      ],
      "include": [
        "server"
      ],
    }
    ```

### STEP 2 | Babel 설정
* babel preset관련 의존성 설치
    ```shell
    > yarn add -D @babel/node
    > yarn add -D @babel/preset-env
    > yarn add -D @babel/preset-typescript
    ```
* babel.config.json 설정 (혹은 .babelrc도 가능)
    ```json
    {
      "presets": [
        [
          "@babel/preset-env",
          {
            "targets": {
              "edge": "17",
              "firefox": "60",
              "chrome": "67",
              "safari": "11.1"
            },
            "useBuiltIns": "usage",
            "corejs": "3.6.5"
          }
        ],
        "@babel/preset-typescript"
      ]
    }
    ```

### STEP 3 | 스크립트 수정
* 이 부분에서 계속 안되서 삽질을 했는데... babel 깃허브에 저렇게 `--extension '.ts'`를 키워드로 써주니 바로 해결되었다. [참고](https://github.com/babel/babel/issues/9301#issuecomment-466685472)
  ```json
  {
    // ...
    "scripts": {
      "server": "nodemon --exec babel-node ./server/index.ts --extensions '.ts'",
    }
    // ...
  }
  ```