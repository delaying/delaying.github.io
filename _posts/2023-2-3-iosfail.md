---
title: Error - expo v47.0.12 
author: delaying
date: 2023-2-3 9:34:00 +0900
categories: [Develop, TrobleShooting]
tags: [React-Native, TrobleShooting]
published: true
---


## 발생 원인
예전에 같은에러가 발생했을 때 패키지 프로그램을 yarn으로 재설치하면 해결됐어서 재설치했는데 계속 새로운 에러가 발생했다.

## 발생 에러
```
iOS Bundling failed 121ms
Unable to resolve "../environment/DevLoadingView" from "node_modules/expo/build/launch/withDevTools.js"
```

```
iOS Bundling failed 121ms
Unable to resolve "../environment/DevLoadingView" from "node_modules/expo/build/launch/withDevTools.js"
```

```
iOS Bundling failed 121ms
Unable to resolve "../environment/DevLoadingView" from "node_modules/expo/build/launch/withDevTools.js"
```

```
iOS Bundling failed 20ms
Unable to resolve "expo-constants" from "node_modules/expo/build/Expo.fx.js"
```


## 에러 해결과정
1. yarn 으로 패키지 파일 재설치를 4번반복했으나 계속 새로운 에러가 떴다.
2. 구글링결과 expo버전을 다운그레이드하면 해결된다고 해서 `"expo": "~46.0.16"`버전으로 다시 패키지를 설치하니까 에러없이 실행되었다.


## 결과
최신 버전에는 에러가 많을 수 있어서, 안전한 버전을 사용하는게 좋다.
