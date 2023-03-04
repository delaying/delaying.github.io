---
title: Native Module, New Architecture, Hermes
author: delaying
date: 2023-3-2 16:04:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

# React Native 동작원리

## Thread

실행되는 프로세스 내에서 실제로 작업을 실행하고, 명령어를 실행하여 처리하는 주체이다.

## React-Native Thread

- Main Thread or UI Thread : Native 영역에 레이아웃을 그려줌
- JavaScript Thread : 작성한 Javascript가 실행되는 곳
- Native Module Thread : Native Module을 다룰 때 사용하게 되는 Thread, 특정 리소스 등을 요청하고자 할 때 사용
- Shadow Thread : virtual DOM으로부터 새로운 Layout으로 변환하도록 계산 해주는 역할

## 동작 과정

- app실행 시 Main Thread에서 Javascript Thread 시작
- Javascript Thread에서 Diffing작업 시작
  - Diffing : virtual DOM과 실제 DOM element를 비교하며 변경되었는지 체크
- 위 과정 끝나면 shadow Thread로 넘어가며 계산 후 main thread로 계산된 layout을 반영함

## React-Native Bridge

- javascript와 native가 서로 소통할 수 있도록 돕는 역할
- json으로 변환하여 소통

# Native Module

주로 현재위치, wifi상태 등 native영역에서만 알고 있는 정보에 접근하는 것 또는 image processing처럼 연산이 native의 높은 performance가 필요할 때 사용한다.

- Native Module
  - 로직/연산에 대한 Native Module
  - 어떤 Native Library의 함수를 호출할 때 사용
- Native UI Component
  - View에 대한 Native Module
  - 주로 카메라 등 연산이 많은 View에 대해 작성
- 동작과정
  - javascript에서 React-Native Bridge로 json으로 변환하여 요청
  - ios/android Native에서 json변환 후 결과값을 계산하여 bridge로 결과값을 전달
  - 다시 json을 변환하여 결과를 javascript로 넘김

# New Architecture

## 새로운 아키텍처 도입 이유

Bridge가 가지고 있는 본질적인 문제를 해결하기 위해 도입되었다.

- Bridge가 가진 제한점

  - 비동기처리 : 어떤 작업이 끝날 때까지 다음프로세스를 기다리지 않고 다른 작업을 바로 실행하는 것
    작업이 끝났는지 보장이 되지 않아, 끝났을 때에 별도 처리가 필요함
  - 싱글스레드 : javasript가 싱글스레드에서 동작하기 때문에 Bridge도 싱글 스레드로 동작함
  - 변환 시 드는 과도한 비용 : bridge로 이동하게 될 때 JSON Object변환하는 비용이 큼

- 기존의 Bridge를 버리고, JSI가 해당역할을 대신하도록 수정
  - JSI : javascript Interface, c++객체에 대한 참조를 할 수 있게 해주는 역할

## 장점

- 동기 실행이 가능하게 됨
- 동시성 : 다른 스레드에 있는 함수를 호출할 수 있음
- json object로 변환 하지 않고 c++언어로 통신하므로 Overhead가 줄어듦
- ios, android간 내부 네이티브 모듈 코드 공유가능
- 타입의 안정성 : 자동으로 생성되는 코드레이어에 의해 자동으로 타입을 생성하도록 함

## 구성요소

### Fabric

- new architecture의 새로운 rendering system
- 이전 architecture에서는 UI Manager가 담당하던 부분
- 개선점
  - shadow thread에서 새로운 shadow tree를 계산하던 로직을 c++로직으로 변환 가능하도록 수정
  - onLayout, onMeasure 등 view의 위치, 사이즈 등을 계산하던 로직을 비동기에서 동기 함수로 변환했기 때문에 많은 퍼포먼스 이득이 있음

### Turbo NativeModule

- 기존 architecture에서는 nativeModule로 사용되던 것
- 장점
  - platform 전반적으로 typecheck가 잘됨
  - 플랫폼 별 코드 공유가 쉬움
  - native module lazy loading이 적용됨
    - lazy loading : 최초에 모든 리소스를 로드 하는 것 이 아닌, 필요할 때 로드하는 방식(최초 로드 시 부하를 줄이기 위함)
  - JSI 사용으로 인하여 native와 javascript코드 간 통신이 효율적

### CodeGen

- 3rd-party library에서 제공되는 코드를 인터페이스에 맞게 작성하면, JSI관련 코드들을 만들어주는 것
- 프로젝트 빌드할 때 자동으로 실행

## New Architecture Project 생성

[여기](https://reactnative.dev/docs/next/the-new-architecture/use-app-template)를 참고하여 new architecture 프로젝트를 생성해 보았다.

- `npx react-native init [파일이름]`
- `cd ios` → `pod install` → `cd ..` → `npm run ios && npm run android`
- `cd ios` → `bundle install && RCT_NEW_ARCH_ENABLED=1 bundle exec pod install`
  → metro bundler에서 fabric:true로 찍히면 new architecture가 켜졌다고 볼 수 있음
- android 폴더 → gradle.properties → newArchEnabled를 true로 바꿈 → 실행하면 똑같이 fabric:true로 찍히게 됨

# Hermes

- facebook에서 만든 javascript engine
- bytecode형태로 미리 컴파일하여 저장해둔 뒤 사용
- 앱 최초 로딩 시 jsbundle파일을 읽어와 동작가능한 javascript로 compile하게 됨 → 이 과정이 대략 android기준 4초까지 걸리는 걸로 파악됨
  → 앱 빌드 시간에 parse와 compile 등 필요한 작업을 빌드할 때 하도록 함
  bytecode형태로 미리 컴파일해두면 실행만 시키면 되기 때문에 걸리는 시간이 절반가량 줄어듦
- 사용하는 memory의 감소 (기존 185mb → 136mb)
- AAB/APK 크기감소 (기존 41ml → 22 mb)
- 적용되었는지 확인
  `const isHermes = () ⇒ global.HermesInternal ! == null;`
  - hermes의 장점을 제대로 확인하려면 release build type으로 확인
