---
title: React Native - Animated API 개념
author: delaying
date: 2023-3-28 20:54:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

# React Native 자체 애니메이션

- Scroll gesture → ScrollView, FlatList, ScetionList
- Touch gesture → Pressable, KeyboardAvoidingView
- 그외 components → Activity Indicator, Modal, Alert

# animation API

## Animated components

Animated는 커스텀하기 좋은 애니메이션이다.

1. 움직일 대상 선언
2. 움직이기 전 값 설정
3. 어떤 인터렉션을 줄 지 선언

### Animated를 사용할 수 있는 컴포넌트

- Text, View, Image, ScrollView, FlatList, SectionList
- 기본 컴포넌트 사용

```
import { Animated } from 'React-native";

<Animated.Text />
<Animated.View />
```

- 애니메이션으로 만들지 못하는 컴포넌트 강제로 애니메이션화하기 (기본컴포넌트로 충분하긴함)

```
import { Animated, Button } from 'React-native";

const AnimatedButton = Animated.createdAnimatedComponents(Button);

<AnimatedButton />
```

### Animated.Value

- 애니메이션 선언과 핸들링 역할을 함
- 초기값 설정하기

```
import React, { useRef } from "react";
import { Animated } from "react-native";

const someAnim = useRef(new Animated.Value(1)).current;
```

### 다른 애니메이션 함수

- setValue(); - 초기값 재설정
- addListener(callback); - 실시간으로 value확인가능
- removeAllListener(); - 함수삭제, 메모리누수를 위해 사용하지 않을때는 삭제할것
- stopAnimation(); - 애니메이션을 멈추는 함수
- resetAnimation(); - 애니메이션을 멈추고 초기값으로 돌아가는 함수
- setOffset(); - gesture들어갈 때 사용됨 추가로 offset설정가능
- flattenOffset();
- extractOffset();

```
import React, { useEffect, useRef } from "react";
import { Animated, Button } from "react-native";

//왼(-100) -> 오(100) 으로 x 값이 변화하는 애니메이션
export default () => {
  const translateAnim = useRef(new Animated.Value(-100)).current;

  useEffect(() => {
    return () => translateAnim.removeAllListeners();
  });

  const onButtonPress = () => {
    translateAnim.setValue(-100);
    translateAnim.addListener(({ value }) => console.log(value));

    setTimeout(() => {
      translateAnim.stopAnimation();
    }, 500);

    Animated.timing(translateAnim, {
      toValue: 100,
      duration: 1000,
      useNativeDriver: true,
    }).start();
  };
  return (
    <>
      <Button title="움직이기" onPress={onButtonPress} />
      <Animated.Text
        style={ fontSize: 70, transform: [{ translateX: translateAnim }] }
      >
        ㅎㅇ
      </Animated.Text>
    </>
  );
};
```

## Animated 인터렉션

- 제공하는 인터렉션
  - Animated.timing : 가장 대중적이고 쉽게 접하는 애니메이션
  - Animated.spring : 선택 옵션이 다양해서 여러 스프링 모델을 구현할 수 있는 spring 애니메이션
  - Animated.decay : 감소하는 값을 이용한 애니메이션

### Animated timing → Easing

- timing config에 들어가는 옵션
- [easing 액션](https://easings.net/) 사이트에서 easing 액션들 확인가능
- in, out, inOut에 따라 변화가 있음
- Easing 진행방향 모듈
  - Easing.in(easing)
  - Easing.out(easing)
  - Easing.inout(easing)
- Easing 액션방식 모듈
  - Easing.back(얼마나 뒤로갈지 값)
  - Easing.bounce
  - Easing.ease
  - Easing.elastic(어느정도 넘치게 bounce될지의 값)
- Easing 함수기반의 액션방식 모듈
  - Easing.linear
  - Easing.quad
  - Easing.cubic
  - Easing.poly
  - Easing.bezier
  - Easing.circle
  - Easing.sin
  - Easing.exp

```
const onButtonPress = () => {
    translateAnim.setValue(-100);
    Animated.timing(translateAnim, {
      toValue: 100,
      duration: 1000,
      delay: 0,
      **easing: Easing.in(Easing.elastic(2)),**
      useNativeDriver: true,
    }).start();
  };
```

→ 시작은 빠르고 뒤로 갈수록 느린게 뒷행동 추측가능하므로 circle을 추천

### Animated.spring

spring offset을 활용한다

```
const onButtonPress = () => {
    Animated.spring(translateYAnim, {
      toValue: 100,

      //   bounciness: 8, // 탄력제어
      //   speed: 12, //속도

      //   friction: 7, //얼마나 빨리 에너지를 소비하는지 -> 값을 낮출수록 탄성생김
      //   tension: 40, //스프링이 얼마나 많은 에너지를 가졌는지

      //   stiffness: 100, //스프링의 강도
      //   damping: 10, //마찰력
      //   mass: 1, //용수철 끝에 매달려있는 물체의 질량

      velocity: 0, //스프링에 부착된 물체의 초기속도 -> 초기속도를 주면 조금 더 자연스러워짐
      useNativeDriver: true,
    }).start();
  };
```

- bouniness, speed
- friction, tension
- stiffness, damping, mass

위 세 그룹으로 나누어서 그룹끼리만 사용해야 액션 작동가능하다(위 숫자값이 초기값)

### Animated.decay

- 오직 속도를 컨트롤함
- 사용자의 제스쳐가 있을때 많이 사용됨

```
const onButtonPress = () => {
  translateAnim.setValue(-100);
  Animated.decay(translateAnim, {
    velocity: 1, //초기속도
    deceleration: 0.997, //감속률
  }).start();
};
```

## Animated 기능

### 결합함수

2개 이상의 애니메이션에서 그룹으로 묶거나, 동기 형식으로 작동되게 하는 함수

- sequence() : 비동기 애니메이션을 동기로 작동하게 해주는 함수
- delay() : 결합함수에서만 사용할 수 있는 함수
- parallel() : 다수의 애니메이션을 그룹으로 묶어주는 함수
- stagger() : 다수의 애니메이션 사이 일관된 delay를 줄 수 있는 함수

아래처럼 작성시 비동기로 작동하여 대각선으로 이동하게됨

```
const onButtonPress = () => {
    Animated.timing(translateYAnim, {
      toValue: 0,
      useNativeDriver: true,
    }).start();
    Animated.timing(translateXAnim, {
      toValue: 100,
      useNativeDriver: true,
    }).start();
  };
```

다음처럼 sequence로 묶어서 작성하면 동기적으로 작동됨

```
const onButtonPress = () => {
  Animated.sequence([
    Animated.timing(translateYAnim, {
      toValue: 0,
      useNativeDriver: true,
    }),
    Animated.timing(translateXAnim, {
      toValue: 100,
      useNativeDriver: true,
    }),
  ]).start();
};
```

### 나머지 메소드

- 사칙연산 메소드 : add, subtract, divide, multiply
  - toValue에서 활용가능
  - Animated.Value값을 합치거나 할때는 사칙연산 메소드를 사용해야함
- 핸들러 메소드 : start, reset, loop

  ```
  const onPressButton = () => {
    Animated.timing(opacityAnim, {
      toValue: 0,
      useNativeDriver: true,
    }).start((finish) => console.log(finish));
  };
  ```

  - reset은 같은 함수를 다시 작성 해야해서 많이 사용되지 않고, resetAnimation() 함수를 사용하는게 더 나음
  - loop : loop 함수안에 반복할 Animated를 작성, iterations로 반복 횟수를 지정할 수 있음

  ```
  const onPressButton = () => {
    Animated.loop(
      Animated.timing(opacityAnim, {
        toValue: 0,
        useNativeDriver: true,
      }),
      { iterations: 3 }
    ).start();
  };
  ```

### 애니메이션 property와 보간법

- property
  - 애니메이션을 줄 수 있는 스타일 속성
  - opacity, transform(x,y,scale)등
  - useNativeDriver값이 true일땐 크기 조절이 불가능하지만, false로 설정 시 property제한을 풀 수 있음
- 보간법

  - 미리 선언해둔 animation.value에 interpolate의 inputRange와 outputRange를 작성하면 됨

  높이가 100에서 200으로 변할 때, 색상은 25f에서 a2d로 변하고, 150일땐 red, 회전하는 예제

  ```
  <Animated.View
          style={
            width: 100,
            height: heightAnim,
            backgroundColor: heightAnim.interpolate({
              inputRange: [100,150, 200],
              outputRange: ['#25f','red' '#a2d'],
            }),
            transform: [
              {
                rotate: heightAnim.interpolate({
                  inputRange: [100, 200],
                  outputRange: ['0deg', '360deg'],
                }),
              },
            ],
          }
        />
  ```
