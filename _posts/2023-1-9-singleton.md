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


```
    const obj1={ n1:1 }
    const obj2={ n2:2 }

    console.log(obj1===obj2);//false
```
이 자체로도 어느정도 싱글톤 패턴이라 볼수 있지만 보통 다음과 같이 구성된다.

```
    class Singleton{
        constructor(){
            if(!Singleton.instance){
                Singleton.instance = this
            }
            return Singleton.instance
        }
        getInstance(){
            return this.instance
        }
    }
    const a = new Singleton();
    const b = new Singleton();
    console.log(a===b)//true
```

## 싱글톤 패턴의 단점
싱글톤 패턴은 TDD(Test Driven Development)를 할 때 나타난다.

TDD를 할때 단위 테스트를 주로하는데, 단위 테스트는 서로 독립적이고 어떤 순서로든 테스트를 실행할 수 있어야 한다.

그러나 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기 어렵다.