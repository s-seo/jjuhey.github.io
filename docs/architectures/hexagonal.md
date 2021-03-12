---
layout: post
title: Hexagonal Architecture
parent: 아키텍쳐(Architecture)
nav_order: 2
last_modified_date: '2021-03-12 23:20'
---

# Hexagonal Architecture
**육각형 아키텍쳐**

우선 어떻게 계층형 아키텍쳐를 보완할 것인가? 에서 출발해보자. 

일단 우리가 계층 구조에서 어플리케이션을 3 계층으로 나눴는데, 이 계층을 잘 살펴보면 사실 DataBase도 외부요소이고, Client도 외부 요소이다. 우리가 설계하려는 웹 어플리케이션은 이 외부 요소들과 잘~ 통신을 해서 데이터를 왔다갔다 하면 되는 것이다. 그렇기 때문에 이 육각형 아키텍쳐에서 Presentation Layer와 Persistance Layer는 바깥에 존재한다. 즉 Outer layer라고도 볼 수 있다. 반면에 우리에게 가장 핵심적인 기능을 구성하고 있는 Business Layer는 이 육각형 구조의 가장 중앙부, Core Layer 혹은 Inner Layer라고 볼 수 있다.

![hexagonal Architecture](../../../assets/images/architecture_hexa.png)

## Outer Layer
**Low Level Layer**

바깥쪽 계층은 저수준의 레이어들이 위치하는 곳이다. 저수준이란, 외부 라이브러리 혹은 외부 요청과 직접 닿아있으며 비지니스 로직을 처리하기 보다 비지니스 로직에 전달하거나 전달 받거나 하여 구체적인 행위를 취하는 레이어이다. 그렇기 때문에, 앞서 배운 Layered Architecture에서 클라이언트의 요청/응답을 처리하는 Presentation Layer, 데이터베이스에 직접 CRUD하는 Persistance Layer와 같은 레이어가 여기에 포함된다.

그래서 두 레이어 모두 Hexagonal Layer에서는 이 바깥 영역, Low Level Layer 라고 불린다. 하지만 이 두가지 레이어는 다음과 같이 나눌 수 있다.

1. Inbound: Presentation Layer
2. Outbound: Persistance Layer

이미 우리는 Layered Architecture에서 두 레이어의 차이점을 배웠기 때문에 더이상 설명하지 않겠다.

* * *

## Core Layer
**High Level Layer**

내부(Inner) 혹은 핵심(Core) 계층은 고수준의 레이어가 위치하는 곳이다. 고수준이란 어플리케이션의 핵심 비지니스 로직을 가지고 있는 레이어이다. 또한 외부와 어떤 직접적인 통신을 하지 않는다. Business Layer, Service 등으로 불린다.

이때 외부와 직접적인 통신을 하지 않고 더 나아가 low level layer에도 의존적이지 않아야 한다는 것이 이 Hexagonal architecture의 주요 컨셉이다.


* * *

## Adapter & Port

핵심 계층과 바깥 계층의 의존성을 제거하기 위해서 알아야 할 개념이 Adapter/Port Pattern이다. 위의 그림에서 초록색으로 표현된 영역인데, 이 개념이 육각형 아키텍쳐에서 가장 중요한 개념이다. 구체적으로 어떻게 어플리케이션에 적용될 수 있는지 알아보자.
* **BEFORE**

```javascript
const userService = (name, age, address) => {
    const user = new User(name, age, address)
    user.setRole('guest')

    return database.create(user)
}
```
위의 코드를 보면, database에 의존적이라는 것을 알 수 있다. 간단히 생각해서, database를 바꾸기 위해서는 서비스에 모든 database를 다른 데이터베이스로 바꿔야 한다. 이게 곧 service가 데이터베이스에 의존적이라는 의미이며 육각형 구조는 이 의존성을 줄이기 위해 Adapter/Port를 이용한다.

어떻게 Adapter/Port를 이용할 수 있는지 살펴보자.

* **AFTER**

```javascript
// Business Layer
const userService = (name, age, address) => {
    const user = new User(name, age, address)
    user.setRole('guest')

    return userPort.createUser(user)
}
```
비지니스 레이어에서는 구체적인 database를 부르는 대신 추상화된 userPort를 부른다.

```javascript
// Port
interface UserPort {
  createUser(user)
  updateUser(userId, change)
  selectUser(userId)
  removeUser(userId)
}

// Adapter
class UserAdapter implements UserPort {
  readonly dataBase
  constructure(dataBase) {
    this.dataBase = dataBase
  }

  createUser(user) {
    return this.dataBase.create(user)
  }
}
```
Port는 인터페이스, Adapter는 Port를 implements하여 구체적인 dataBase를 주입받아 기능을 구현한다.

실제로 적용할 때에는 runtime에서 Adapter를 바인딩해줄 수 있는 DI(Dependency Injection) container를 구현하거나 DI library를 이용할 수 있다.
`const userPort = new UserAdapter(mongoDB)` 이런식으로 주입해줘야 한다. 자세한 이야기는 DI에 대해 이야기해볼 때 다뤄보겠다.


정리해보자면,
1. Port: 추상화된 인터페이스. 비지니스 계층에서는 이 포트를 가져다 쓴다.
2. Adapter: Port를 상속받아 구현한 구현체. 여기에서 Outer layer를 직접 구현하고 가져다 쓴다.
  * Inbound Adapter: Presentation layer와 연결해주는 어뎁터
  * Outbound Adapter: Persistance layer, 외부 엔진(가져다 써야하는!!)을 연결해주는 어뎁터

육각형 구조로 Adapter/Port를 적용하면 2가지 이점을 가진다.
1. **유지보수가 용이하다.**
  * DB 종류를 바꾼다는 등의 변경사항이 있을 때, 생성자에 의존성 주입을 하는 DB만 교체하면 된다.
2. **테스트 코드 작성이 쉬워진다.**
  * 마찬가지로 생성자에 의존성 주입을 하는 '테스트용' 목업 DB만 만들어서 교체하면 되기 때문에 테스트 코드 작성도 매우 용이해진다.


* * *

## 특징 정리
1. **느슨한 결합(Loosing Coupling)**: Adapter/Port pattern을 이용하여 Core Layer와 Outer Layer의 의존성을 줄여서 **유지보수** 및 **테스트가 용이**하도록 한다.
2. **DIP(Dependency Inversion Principle, 의존성 역전 원리)**: 육각형 아키텍쳐와 클린 아키텍쳐에서 중요하게 다루는 또다른 중요한 개념이다. 이 개념에 대해서는 클린 아키텍쳐에 대해서 다룬 후에 추후에 다루기로 하겠다.
