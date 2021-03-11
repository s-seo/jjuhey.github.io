---
layout: post
title: Layered Architecture
parent: 아키텍쳐(Architecture)
nav_order: 1
---

# Layered Architecture
**계층형 아키텍쳐**

계층형의 기본적인 컨셉은 MVC pattern과 유사하다. Model, View, Contoller가 각각 레이어로 볼 수 있으며, 각각의 레이어는 특정한 "역할"을 수행하는 것에 있어서 그 핵심적인 개념이 같다.

![Layered Architecture](../../../assets/images/architecture_layered.png)

구글링을 해보면 많은 문서들이 3 레이어, 4 레이어 등등 다양하게 설명한다. MVC pattern과 혼동되게 설명놓은 곳도 많다. 물론 비슷한 점이 많지만, MVC는 내가 생각하기에 Java Web Application에 맞춰서 설계한 디자인 패턴에 가깝다. Layered Pattern은 이보다 더 큰 관점에서 아키텍쳐를 관찰하는 것이고, 그렇기 때문에 다양한 어플리케이션에 적용될 수 있다.

나 같은 경우는 Nodejs, typescript, express를 사용하기 때문에 이러한 MVC pattern은 적용하기가 조금 애매한 감이 있다. 그래서 Layered Architecture의 큰 그림을 보고 어떻게 이 개념을 구체적으로  Nodejs Web Application에 적용할지를 바라봐야 할 것이다.

계층형 아키텍쳐의 핵심은 **관심사의 분리**이다. 각 레이어는 특정한 관심사(혹은 역할)을 가지고 있으며, 서로 어떤 관심사를 가지고 있는지는 전혀 알지 못한다.
마치 내가 내 어떤 업무를 시킬 사람을 고용하고 싶은데, 그 사람이 어떤 관심사가 있는지 어떤 취미가 있는지에 대해서는 전혀 알 필요가 없는 것 처럼 말이다. (예시가 좀 이상한가...?🤔)

* * *

## Presentation Layer
**표현 계층, User Interface, View**

클라이언트와 직접적으로 맞닿아있는 레이어이다. 가장 쉬운 예제는 사용자 인터페이스이다. 사용자가 직접 보고 요청을 하고 응답을 받기 때문이다.

Back-end관점에서는 어떨까?
백엔드 관점에서는 MVC Pattern에서 Controller에 해당한다. 엄밀하게 말해서 컨트롤러가 '표현'하는 계층이 맞나? 라고 한다면 모르겠다. 하지만 적어도 외부와 가장 맞닿아있는 계층이자 백엔드의 관점에서는 클라이언트가 프론트엔드이기 때문에 어느정도 상통하는 이야기라 생각된다.
```javascript
const controller = (req, res) => {
  const data = req.body;
  try {
    const result = await service(data)
    return res.json({ success: true, result })
  } catch (error) {
    return res.json({ success: false })
  }
}
```
* 클라이언트로 부터 들어온 요청/응답 처리, 서비스 호출 그리고 에러 핸들링

* * *

## Business Layer
**비지니스 계층, Service, Domain, Core**

이름처럼 비지니스 로직을 처리하는 레이어이다. 또 다른 이름으로는 Service, Domain 등으로 부를 수 있다. 어플리케이션의 핵심적인 기능을 구현하는 레이어로, 어떻게 표현되는지(Presentation), 데이터 베이스와는 어떻게 통신하는지(Persistance)에는 관심이 없어야 한다.
```javascript
const service = (title, content) => {
  const document = new Document(title, content)
  const result = await persistance(document)
  if (!result.data) throw Error('no data')

  return result
}
```
* 트랜젝션 단위의 일 처리 및 영속성 계층과 통신, 에러를 던짐

* * *

## Presistance Layer
**영속성 계층, Repository, DAO**

영속성 계층은 DataBase와 직접 통신하는 레이어이다. Repository pattern이나 MVC패턴의 DAO과 동일하다. 기본적으로 가장 원자단위의 일을 처리하며 그 일은 CRUD(Create, Read, Update, Delete)라고도 한다.
```javascript
  const persistance = (document) => {
    return DataBase.save(document)
  }
```
* CRUD 단위의 일을 처리, 데이터베이스와 직접 연결됨

(위의 코드들은 예제를 위해 간이로 작성된 코드입니다. 실제 프로젝트에 적용하는 예제가 아님을 명심해주세요.)

* * *

## 특징 정리
1. **관심사의 분리 (Separation of concern)**: 각각의 레이어는 모두 특별한 역할이 있으며, 서로 다른 레이어의 역할에 관심을 갖지 않는다.
2. 추가적으로 Domain Model(Entity, VO, DTO) layer, Infrastructure layer, Application Layer 등이 있으나, 이후에 나올 다른 아키텍쳐에도 비슷한 개념이 나오며 Layered Architecture의 개념을 조금 헷갈리게 할 수 있으므로 생략하였다.






