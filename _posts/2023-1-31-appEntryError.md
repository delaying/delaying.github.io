---
title: Error - Unable to resolve module ./node_modules/expo/AppEntry from ""
author: delaying
date: 2023-1-31 10:42:00 +0900
categories: [Develop, TrobleShooting]
tags: [React-Native, TrobleShooting]
published: true
---

## 발생 원인
react-native 프로젝트인 [계산기](https://github.com/delaying/ReactNative-study/tree/main/calculator) 앱 프로젝트를 진행하면서 하나의 프로젝트 폴더에 여러개의 프로젝트 폴더들을 생성하면서부터 에러가 발생하였다.

## 발생 에러
프로젝트 내에서 `npx expo start`명령어로 실행하면 다음과 같은 에러가 발생했다.
```
Error: Unable to resolve module ./node_modules/expo/AppEntry from ""
```

## 에러 해결과정
1. 구글링을 해본 결과 폴더내부에 **App.js** 파일이 있는지 확인하는 것이 1순위였다.

    `npx create-expo-app` 으로 프로젝트를 시작해서 App.js파일은 자동으로 프로젝트에 들어가 있었다.

2. **node_modules**파일을 삭제하고 다시 설치하는 것이다.
    - 먼저 `rm -rf node_modules` 터미널에 입력하여 기존의 node_modules 폴더를 제거한다.
    - `yarn` 이나 `npm install`로 다시 node_modules를 재설치한다.


## 결과
나는 node_modules 재설치시에 `npm install`명령어로만 설치했었는데, 계속 같은 에러가 반복됐었다.

새로 프로젝트폴더를 만들어도 똑같아서 node_modules 재설치할 때 `yarn`으로 설치했는데 에러가 바로 해결되었다.


이 에러를 1시간은 붙잡고 있었는데, 왜 사람들이 yarn을 많이 사용하는지 알 것 같다.....

