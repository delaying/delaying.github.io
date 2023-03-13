---
title: Error - Unable to resolve module ./node_modules/expo/AppEntry from ''
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

새로 프로젝트 폴더를 만들어도 똑같아서 node_modules 재설치할 때 `yarn`으로 설치했는데 에러가 바로 해결되었다.

npm과 yarn의 차이점을 찾아보았다.

### npm과 yarn의 차이

`npm install` 명령어를 통해 모듈들을 설치하면 해당 메이저 버전 중 최신 버전을 다운받는다.
이럴 경우 사용하는 모듈간의 버전 불일치로 인한 문제가 발생할 수도 있다.

그리고 npm은 패키지를 찾지 못하면 상위 디렉토리의 node_modules 폴더를 계속 검색한다.
상위 디렉토리가 어떤 node_modules를 포함하고 있는지에 따라 다른 버전의 의존성을 잘못 불러올 수도 있다.

yarn은 사용할 모듈의 버전을 지정하기 위해 프로젝트에 .lock 파일을 포함한다.
정확한 버전을 지정하고 고정하기 때문에 프로젝트를 개발할 때 어느 환경에서도 항상 같은 버전의 모듈을 사용할 수 있도록 한다.

#### 참고

- [NPM vs YARN의 차이점을 알아보자](https://developer0809.tistory.com/128)
- [npm, yarn, 그리고 yarn berry](https://usage.tistory.com/147)
- [npm? yarn? 그 차이가 뭐길래](https://seogeurim.tistory.com/12)
