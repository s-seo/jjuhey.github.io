---
layout: post
title: Socket Hang up
parent: 오답노트(trouble shooting)
nav_order: 1
last_modified_date: '2021-03-16 22:00'
---

## Socket Hang Up

### **Problem**
* 회사 컴퓨터를 새로 바꿔서 개발환경 구축 및 개발자 셋팅을 다시 진행하였다. 그러다가 대부분의 설정이 완료되서 어플리케이션을 구동했는데, 아래와 같은 에러가 발생하였다.
    ```shell
    Error: socket hang up
        at createHangUpError (http.js:1445:15)
        at Socket.socketOnEnd [as onend] (http.js:1541:23)
        at Socket.g (events.js:175:14)
        at Socket.EventEmitter.emit (events.js:117:20)
        at _stream_readable.js:910:16
        at process._tickCallback (node.js:415:13)
    ```
* 이전에 사용하던 컴퓨터에서는 아무 이상없이 실행되었기 때문에, 코드베이스에 버그가 있는 것은 아니었음. 컴퓨터 셋팅 도중에 셋팅 오류가 있었던 것으로 예상이 되었다.
* Possible Cause
    1. Docker - File Sharing
    2. Incorrect Node version
    3. Not install something yet...?
    4. Incorrect Locale & Language setting in 시스템 환경설정 of Mac


### **Solution**
* 최근에 입사하신 회사 동료분도 비슷한 문제를 겪으셔서 다음날 여쭤봤음
* 우리 어플리케이션은 웹으로 구동되고, 그 내부에 machine learning 연산을 하는 module 등 여러가지 모듈이 Docker container로 구동되고 있다.
* 그래서 문제의 핵심은 <span class='text-red-000'>container에 할당된 메모리가 부족</span>해서였다.
* `Docker - Preferences - Resources - Memory`: 2GB 👉 8GB


### **Learn Point**
* 이전에 분명 봤었던 문제였던거 같은데.. 오답노트 작성 꼭 하자.
* 도커에 문제가 있어도 어쨌든 우리쪽 서버에서 에러가 나온다. 주의하자!
* 초기에 로그인하고 들어갔을 때, system info가 제대로 찍혀나오지 않으면 뭔가 셋팅이 이상하다고 의심해보자.