---
title: React Native - Navigation
author: delaying
date: 2023-2-13 16:08:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

[react-navigation](https://reactnavigation.org/docs/getting-started)은 react-native에서 사용하는 화면이동을 위한 라이브러리이다.

navigator와 screen으로 구성되어 있다.

## 종류

### stack Navigator

자료구조 중 stack은 제일 마지막에 들어온 게 제일 먼저 나가는 방식이다.

이 stack Navigator또한 제일 마지막에 켜진 화면이 제일먼저 사라진다.

- stack navigator : js로 작성하며 자유도가 높다.
- native stack navigator : native로 작성해야하고, 자유도가 낮다.

### drawer Navigator

슬라이드를 통해 이동할 screen들을 나타낸다.

open, close,toggle 등의 기본적인 기능들을 함수로 제공한다.

### tab Navigator

앱에서 사용하는 가장흔한 ui이다.

기본적으로 하단의 탭 형태로 제공된다.

## 기본 용어

- navigator : 화면을 어떻게 그려줄지 결정해주는 리액트 컴포넌트
- Router : navigation의 상태나 동작을 제어해주는 함수의 집합. 주로 화면간 데이터 전달을 위해사용
- screen : 화면을 그려주는 컴포넌트
- navigation prop : 화면이동에 대한 함수들을 공통으로제공
- route prop : 파라미터를 받아오거나 어떤 화면인지 이름을 알 수 있는 값들을 제공 해주는 역할
- navigation state : 현재 react navigation이 어떤 상태인지를 알 수 있는 값, 주로 stack navigator 이전 stack에 어떤 screen이 있는지 찾기위해사용됨
- route : screen의 name,key,param 등을 저장하는 개념, 어떤화면인지 식별하기위해 사용, navigation state하위에 routes라는 배열에서 찾을 수 있음.

## stack navigator

presentation : stack navigator에서 화면 이동 애니메이션에 대한 설정 옵션이다.

- card : 오른쪽에서 왼쪽으로 이동하는 애니메이션
- modal : 아래에서 위로 이동하는 애니메이션

## tab navigator

backBehavior : android에서 H/W button을 눌렀을 때 어떻게 이동하는지 지정가능한 옵션이다.

- fireRoute : 선언상 제일 처음에 있는 탭으로 이동
- initialRoute : 최초 지정한 탭으로 이동
- order : 탭을 선언한 순으로 이동
- history : 이동한 히스토리 역순으로 이동

## nesting navigator

navigator의 screen을 component가 아닌 다른 navigator로 선언하는 것이다.

주로, presentation을 다르게 선언하거나, 조건에 따라 navigator의 분기가 필요할 때 사용된다.

## DeepLink

특정 URL을 누르면 지정한 화면으로 이동한다.

앱마다 유용한 scheme을 가지고 있는데, 웹으로 치면 https://가 scheme부분이다.

- universal link(ios), app links(android)
  무단으로 scheme을 빼앗아가는 현상을 막기위해 각각의 플랫폼에서의 도메인 인증단계를 통과해야한다.

## CommonAction

- navigator : 특정화면으로 이동하는 action
- reset : 현재상태를 지정한 상태로 변경해주는 action
- goBack : 이전 히스토리로 이동해주는 action

## stackAction

- push : 새로운 화면을 최상단에 넣는 것
- pop : 현재 보이는 화면을 꺼내는 것

## tabAction

- jumpTo : 탭간 이동 해야할 때 사용됨

## 활용

`npm install @react-navigation/native`로 설치한다.

react navigation은 전체용량을 줄이기 위해 각각의 패키지가 분리되어 있다.

사용하려면 따로따로 설치해야한다.
`npm install react-native-screens react-native-safe-area-context`을 실행하고,

expo 사용시엔 `npx expo install react-native-screens react-native-safe-area-context` 추가 dependencies를 설치해야한다.

```
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>{/* Rest of your app code */}</NavigationContainer>
  );
}
```

### native stack 사용

`npm install @react-navigation/native-stack`

기본으로 작성해야할 내용이다.

```
import * as React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import ScreenA from "./src/ScreenA";
import ScreenB from "./src/ScreenB";
import NestedStackNavigator from "./src/NestedStackNavigator";

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="ScreenA" component={ScreenA}></Stack.Screen>
        <Stack.Screen name="ScreenB" component={ScreenB}></Stack.Screen>
        <Stack.Screen
          name="Nested"
          component={NestedStackNavigator}
        ></Stack.Screen>
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

스크린 컴포넌트에 작성할 내용이다.
onPress함수에서 navigation.navigate를 사용하면된다.

```
//ScreenA.js
import { Button, Text, View } from "react-native";
import ScreenC from "./ScreenC";

export default ({ navigation }) => {
  return (
    <View style={\{ flex: 1, alignItems: "center", justifyContent: "center" }\}>
      <Text>ScreenA</Text>

      <Button
        title="B스크린으로 이동"
        onPress={() => {
          navigation.navigate("ScreenB", { value: "fromA" });
        }}
      />

    </View>
  );
};
```

또한 다른 컴포넌트에 value값으로 값을 넘겨줄 수 있다.

```
//ScreenB.js
import { Button, Text, View } from "react-native";

export default ({ route }) => {
  return (
    <View style={\{ flex: 1, justifyContent: "center", alignContent: "center" }\}>
      <Text>B스크린 A에서받은값{route.params.value}</Text>
      <Button
        title="뒤로"
        onPress={() => {
          console.log("gg");
        }}
      />
    </View>
  );
};
```

### tab navigation

`npm install @react-navigation/bottom-tabs`

기본 형식이다.

```
import * as React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import TabA from "./src/TabA";
import TabB from "./src/TabB";

const Stack = createNativeStackNavigator();
const BottomTab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <BottomTab.Navigator>
        <BottomTab.Screen name="TabA" component={TabA} />
        <BottomTab.Screen name="TabB" component={TabB} />
      </BottomTab.Navigator>
    </NavigationContainer>
  );
}
```

하단 아이콘을 [expo-icons](https://icons.expo.fyi/)로 변경할 수 있다.
옵션에 값을 넣으면된다.

```
<BottomTab.Screen
          name="TabB"
          component={TabB}
          options=\{\{ tabBarIcon: () => <Ionicons name="settings" size={20} /> \}\}
        />
```
