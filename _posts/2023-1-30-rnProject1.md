---
title: React-Native 01 - 카카오톡 친구목록 클론코딩
author: delaying
date: 2023-1-30 11:00:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

[까까오톡 친구목록 클론코딩](https://github.com/delaying/ReactNative-study/tree/main/kakao-friend-list) 프로젝트를 진행하면서 알게된 내용들을 작성하였다.

## 프로젝트 실행
Expo CLI를 사용한 프로젝트이므로 `npx create-expo-app <프로젝트이름>`명령어로 프로젝트를 생성한다.

생성한 프로젝트 폴더 내에서 `npx expo start`명령어를 실행하여 핸드폰 Expo Go 어플을 설치하고 바코드를 인식하면 결과를 바로바로 확인할 수 있다.


## 간단한 작성법
컴포넌트를 만들 때 매번 다음처럼 작성했었다.
```
const App = () => {
	return()
}

export default App;
```

그러나 파일이름과 컴포넌트 이름이 같다면 위 코드를 줄여서 다음처럼 간단하게 작성할 수 있다.
```
export default () => {
	return()
}
```

또한, default 를 붙이면 대표적인 것 하나만을 의미하기 때문에 import 할 때 구조분해할당{}을 사용할 필요가 없다.


## SafeAreaView
각 핸드폰마다 상단바와 하단바가 차지하는 영역이 다르므로 유동적으로 관리해줄 라이브러리가 필요하다.

- react-native 자체의 SafeAreaView 사용
- react-native-safe-area-context 라이브러리에서 SafeAreaView 사용,  edge옵션으로 세부적으로 설정가능

## Expo icons
아이콘 삽입 시 expo에 포함된 icons 라이브러리를 활용하면 된다.

[expo-icons 페이지](https://icons.expo.fyi/)에서 원하는 아이콘을 찾아서 다음처럼 사용하면된다.

```
import { Ionicons } from '@expo/vector-icons';

export default () => {
	return(
        <View>
            <Ionicons name="search-outline" size={24} color="black" />
        </View>
    )
}
```

## TouchableOpacity
친구목록 화살표 view를 터치에 응답하도록 하는 래퍼인 TouchableOpacity로 감싼다. 

`activeOpacity`속성은 터치 시에 깜빡이는 정도를 설정할 수 있다.


또한,편리한 사용과 다른 기능을 방해하지 않도록 터치범위를 조정해주어야 한다.
- margin이나 padding 값 조절
- [hitSlop](https://reactnative.dev/docs/pressable#hitslop)속성 사용하면 더 쉽게 조절가능함
    `hitSlop = {{top:10, bottom:10}} `

### Button을 사용하지 않는 이유
`<Button/>` 컴포넌트는 안드로이드와 ios에서 다르게 보여지므로 관리하는데 어려움이 있다.

그래서 `TouchableOpacity`를 사용하여 터치영역을 만들어준다.

## FlatList
친구목록은 스크롤이 가능하도록 FlatList로 감싼다.

ScrollView를 사용해도 되지만, ScrollView는 데이터 양이 많지않고 고정적일 때 사용하는 것이 좋다.

친구목록은 데이터의 양이 적지않고, 가변적이므로 [FlatList](https://reactnative.dev/docs/flatlist)를 사용하는 것이 좋다.

FlatList는 렌더링시 한번에 모든데이터를 출력하는 ScrollView와 달리 화면에 보여지는 부분만 렌더링하므로 ScrollView보다 성능이 훨씬좋다.


## map()
친구목록의 데이터는 map함수를 사용하여 데이터 길이만큼 컴포넌트를 반환시킨다.
- map함수는 key 값이 있어야 한다.
- key옵션은 map함수 내의 최상단 루트컴포넌트에 있어야한다.


## Styled-Components
CSS를 적용하는 방법은 inline, StyleSheet, [Styled-Components](https://styled-components.com/docs/basics#react-native) 이 3가지를 사용하는게 대표적이다.

inline 스타일링은 앱이 커지면 최적화 측면에서 렌더링 될때마다 새로운 오브젝트가 계속 할당되어 성능에 좋지않다.

이를 보완하는 StyleSheet나 Styled-Components를 사용하면 된다.

개인적으로 StyleSheet보다는 Styled-Comoenents가 가독성이 더 좋고 간편해서 Styled-Components를 선호한다.
- StyleSheet
    ```
    <View style={styles.container}>
    </View>

    const styles = StyleSheet.create({
        container:{
            flex:1,
            backgroundColor:"#fff",
        ),
    })
    ```

- Styled-Components 
    ```
    const Container = styled.View`
	flex:1;
	background-color:#fff;
    `;

    <Container>
    </Container>
    ```