---
title: Xcode로 ios simulator 사용하기
author: delaying
date: 2023-2-7 23:30:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

React Native 앱 개발을 하면서 핸드폰으로만 개발상황을 확인하는 게 불편해서 가상 simulator를 설치하게 됐다.

Xcode는 Mac OS환경에서만 사용할 수 있다.

## expo simulator 실행방법

1. 먼저 [Xcode](https://apps.apple.com/kr/app/xcode/id497799835?mt=12)앱을 설치한다.
2. 앱을 실행하고, 상단 메뉴에서 Xcode -> settings -> Locations를 클릭한다.
3. Command Tool Line의 버전을 다시 한번 클릭한다.
4. 상단 메뉴의 Xcode -> Open Developer Tools -> Simulator를 클릭한다.
5. React Native 프로젝트 폴더에서 `npm start`를 실행한다.
6. 실행 후 `i` 키를 클릭하면 ios simulator에서 실행된다.
7. 아이폰 시뮬레이터에서 expo 앱이 생성되고, 실행되면 성공이다.

## 단축기 & 사용팁

- ios 시뮬레이터 키보드 on/off : cmd + shift + k
- 상단 메뉴의 I/O에서 다양한 I/O기능들을 사용할 수 있다.
- Dock 바에서 Simulator 아이콘 위에서 오른쪽마우스 클릭하면 device를 선택할 수 있다.

## 주의

모든 앱이 시뮬레이터에서 실행되지는 않는다.

다음의 경우엔 기기에서 직접 확인하여야 한다.

- 전화 기능
- 카메라, 가속도 센서, GPS 등의 기기내장 하드웨어 사용
- 프로젝트 코드에서 기기(아이폰, 아이패드 등)에서만 실행가능하도록 되어있을 경우
