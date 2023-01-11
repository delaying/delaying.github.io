---
title: Singleton Pattern
author: delaying
date: 2023-1-9 16:46:00 +0900
categories: [Study, CS]
tags: [CS, Design-Pattern]
published: true
---

## 싱글톤 패턴
싱글톤 패턴(Singleton pattern)은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴이다.<br/>
이를 기반으로 로직을 만드는데 쓰이고, 보통 데이터베이스 연결 모듈에 많이 사용된다.

하나의 인스턴스를 만들고 해당 인스턴스를 다른 모듈들이 공유하여 사용하므로 인스턴스 생성에 드는 비용이 줄다는 장점이 있지만, 의존성이 높아진다는 단점이 있다.

## 자바스크립트에서의 싱글톤 패턴
자바스크립트에서는 리터럴 {} 이나 new Object로 객체를 생성하면 다른 어떤 객체와도 다르므로 이 자체만으로 싱글톤 패턴을 구현할 수 있다.

