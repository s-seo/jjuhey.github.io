---
layout: post
title: JWT(JSON web token)란?
nav_order: 89
last_modified_date: 2021-03-20 23:56
lastmod: 2021-03-20 23:56
---

# **jwt에 대해 알아보기**

## Intro
Nodejs로 Backend를 구현할 때, jwt를 이용해서 토큰을 생성해주게 된다. 회사에서도 이 jwt를 사용하는 것 같은데, 내가 초기 셋팅을 하지 않아서 한번도 다뤄본적은 없다. 그래서 이번에 내 개인홈페이지를 구축할 때, 처음 jwt를 마주하게(?) 되었는데.. 그냥 쓰기보다 제대로 알고서 써보자 싶어서 이렇게 포스팅하게 되었다.

## What is jwt?
> **JSON Web Tokens**: an open standard(RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object.`

정보를 JSON 객체로 안전하게 전송하기 위해 컴팩트하고 독립적인 방식을 정의하는 개방형 표준이라고 한다.
[공식홈페이지](https://jwt.io/introduction)의 설명에 따르면, secret(HMAC algorithm) 혹은 공개키/개인키 쌍(RSA or ECDSA)을 이용하여 사용할 수 있다고 한다.

npm 혹은 yarn으로 설치할 수 있다.
```shell
npm install jsonwebtoken
```

공개키/개인키는 우리가 github에 초기에 등록하는 SSH와 비슷한 원리인 것 같다. SSH도 공개키를 github에 등록해놓고 개인키를 내 로컬에 저장해서, 개인키를 가지고 있는 '나의 PC'가 접근 가능하도록 하는 그런 원리인 듯하다. (더 자세한 원리는 나중에 따로 공부하도록 하고 엣헴)

아무튼 그렇다면 이 웹 토큰은 언제 사용해야 할까?

## When should I use JSON Web Tokens?
1. 승인 (Authorization)
  * 사용자가 로그인 후, 각 요청에 JWT가 포함되어 있다. 이 토큰으로 routes, service, resources 등에 접근을 그 해당 (개인키를 가지고 있는)유저에게 허용한다.
  * 장점: 작은 오버해드, 여러 도메인에서 쉽게 사용될 수 있다.
  * 응용: Single Sign On(SSO)는 JWT를 널리 사용하게 한다.
2. 정보교환 (Information Exchange)
  * 당사자간 정보를 안전하게 전송하는 방법으로 쓰인다.
  * 공개/개인키를 사용하므로, 쉽게 보낸사람이 자신이 말하는 사람인지 확신할 수 있다. (무슨말인지 정확히 모르겠음ㅠ)
  * 서명은 Header, Payload를 이용하여 계산되므로, 컨텐츠가 변조되었는지도 확인할 수 있다.

## JWT structure
3가지의 파트로 이루어져있다.
1. Header
2. Payload
3. Signature
그래서 JWT의 전통적인 구조는 `xxxxx.yyyyy.zzzzz`로 이루어져있다.

### Header
* type, algorithm(HMAC SHA256, RSA)
* example: 
  ```json
  {
    "alg": "RS256",
    "typ": "JWT"
  }
  ```

### Payload
* claim: entity(사용자)와 추가적인 데이터에 대한 설명을 담고있다.
* 3가지 타입: registered, public, private claims
  ```json
  {
    "sub": "1234567890",
    "name": "John Doe",
    "admin": true,
  }
  ```
* 서명된 토큰의 경우, 변조로 부터 보호되지만 누구나 읽을 수 있다. 즉, 암호화되지 않은 경우에 JWT의 payload, header에 비밀정보를 넣어서는 안된다.

### Signature
* Signature를 만들기 위해서는 인코딩된 header, 인코딩된 payload, secret, algorithm(header에 지정된 것)을 가져와야 한다.
* Example: HMAC SHA256을 사용할 경우,
  ```
  HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret
  )
  ```
* 이 서명은 메시지가 변경되지 않음을 확인할 때 사용된다.
* 개인키로 서명된 토큰의 경우, JWT의 발신자가 자신이 말하는 사람인지도 확인할 수 있다.

## How to use jwt on Node.js Web Application?
### sign
* 토큰을 생성할 수 있다.
* default: HMAC SHA256
  ```javascript
  const jwt = require('jsonwebtoken')
  const token = jwt.sign({ data: 'myData' }, 'secretToken');
  ```
* 1 hr of expiration
  ```javascript
  const token = jwt.sign({
    exp: Math.floor(Date.now() / 1000) + (60 * 60),
    data: 'myData',
  }, 'secretToken')
  ```

### verify
* token을 원래 정보로 다시 바꾼다. (decode)
* 이 decode된 정보 & token이 내가 가진 정보와 동일한지 대조할 수 있다.
  ```javascript
  const decoded = jwt.verify(token, 'secretToken')
  console.log(decoded.data) // myData
  ```

### Apply to my Web
1. 로그인할 때, 토큰을 생성해서 저장(DB & frontend)한다.
2. 유저의 요청이 들어올 때마다 미들웨어에서 token이 일치하는지 확인 후 요청을 처리한다.
3. 로그아웃할 때, 토큰을 reset시켜준다.