---
layout: default
title: Hexagonal Architecture (작성중)
parent: 아키텍쳐(Architecture)
nav_order: 2
---

# Hexagonal Architecture
> 육각형 아키텍쳐

우선 어떻게 계층형 아키텍쳐를 보완할 것인가? 에서 출발해보자. 
일단 우리가 3 계층으로 나눴는데, 이 계층을 잘 살펴보면 사실 DataBase도 외부요소이고, Client도 외부 요소이다. 우리가 설계하는 것은 이 외부요소들과 잘~ 통신을 해서 데이터를 왔다갔다 하면 되는 것이다. 그렇기 때문에 이 육각형 아키텍쳐에서 Presentation Layer와 Persistance Layer는 바깥에 존재한다. 즉 Outer layer라고도 볼 수 있다. 반면에 우리에게 가장 핵심적인 기능을 구성하고 있는 Business Layer는 이 육각형 구조의 가장 중앙부, Core Layer 혹은 Inner Layer라고 볼 수 있다.

![hexagonal Architecture](../../../assets/images/architecture_hexa.png)

## Outer Layer
1. Inbound: Presentation Layer
2. Outbound: Persistance Layer



* * *

## Core Layer



* * *

## Adapter & Port
1. Port
2. Inbound Adapter
3. Outbound Adapter

* * *

## 특징 정리
1. 느슨한 결합(Loosing Coupling)
2. DIP(Dependency Inversion Principle, 의존성 역전 원리)
