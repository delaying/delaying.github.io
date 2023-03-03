---
title: 꼭 알아야 할 ios와 android 개념
author: delaying
date: 2023-3-1 15:53:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

# Android

- application : 안드로이드에서 전체 앱상태를 관리하는 class
  event를 전달하기 위한 함수를 제공
  - onCreate : 어플리케이션이 실행될때 최초에 호출
  - onTerminate : 어플리케이션이 종료될 때 호출
- manifest
  - android 앱의 메타 정보를 요약해서 선언한 것
  - 권한, 이름, 패키지명 등 앱의 전반적인 내용들을 담고 있음
  - 위치:android/app/src/main/AndroidManifest.xml
- Activity
  - 안드로이드에서 화면을 구성하는 요소
  - 유저가 직접 보고, 누르는 등의 액션이 발생됨
  - 안드로이드의 4구성 요소(activity, service, receiver, content provider)
  - Intent
    - 어떤 activity를 호출할 때 사용
    - 매개변수와 함께 보냄
    - navigation의 route와 비슷한 형태
  - Activity Life-cycle
    - onCreate → onResume → onPause → onDestroy
  - Intent Filter
    - intent를 실행시킬 때 어떤 종류의 activity인지를 빠르게 찾기 위한 수단
    - 구조
      - action : activity가 어떤 행동에 유효한 것 인지를 나타내는 값
      - category:어떤 종류의 액티비티 인지를 나타내는 값
    - 종류
      - ACTION_MAIN:앱의 시작점, 홈화면에 아이콘이 만들어짐
      - ACTION_SEND:공유하기 액션등이 필요할 때 사용됨
      - ACTION_DIAL:전화번호 폰 패드와 같은 화면이 필요할 때 사용됨

# IOS

- AppDelegate
  - android에서 activity처럼 화면을 구성하는 단위
  - 각각 앱의 상태에 따라 불려지게 되는 함수가 있음
  - ios app status
    - not running → inactive → active → background → suspended
    - suspended는 메모리를 먹지 않도록 os가 앱을 사용하지 않을 때 관리하는 역할
  - didFinishLaunchingWithOptions : 앱이 최초 실행될 때 호출되는 함수
- Info.plist
  - 권한, 앱의 이름, 실행 시 주로 필요한 값들을 관리 해주는 파일
  - SDK API key값, 권한 요청 시 텍스트 등을 관리
- Build phase
  - 앱을 시킴에 있어 필요한 값들을 자동으로 설정하도록 command로 모두 선언 해둔 것
  - pod install시 자동으로 생성됨

# Android / IOS Permissions

특정 리소스를 필요로 할 때 사용자에게 허용할 것인지 물어보는 것

- 권한 획득
  - ios
    - 맨 처음 권한 거절 시 다시 설정하려면 삭제 후 재설치 또는 직접 설정에서 권한을 적용해줘야 함
    - Info.plist권한 요청문 → 해당 권한이 왜 필요한지 작성 해야함
  - android
    - ios와 달리 거절하더라도 나중에 다시 권한을 설정할 수 있음
- 자주 사용하는 권한
  - 사진 관련
    - ios : NSPhotoLibraryUsageDescription
    - android : READ_EXTERNAL_STORAGE
  - 카메라 관련
    - ios : NSCameraUsageDescription
    - android : WRITE_EXTERNAL_STORAGE
  - 위치 관련 권한
    - ios : NSLocationAlwaysAndWhenInUseUsageDescription
    - android
      - ACCESS_FINE_LOCATION
      - ACCESS_COARSE_LOCATION
      - ACCESS_BACKGROUND_LOCATION
  - AppTrackingTransparency
    - IDFA를 광고식별자를 읽어오는 권한
    - 광고를 위해 사용자 정보(연령대, 관심사 등)를 받아옴
    - ios 앱 심사 시 필수로 보고 있는 정보 → 권한 prompt만 띄워도 해결됨

# Android / IOS Scheme

scheme : 외부에서 우리 앱을 호출하거나, 우리 앱이 외부앱을 호출하는 수단

결제 시 많이 사용됨(토스,카카오페이,네이버페이 등)

- 정의
  - android : intent-filter를 통해서 정의
  - ios : info.plist → URL Types에 저장
- 테스트
  - uri-scheme package를 통하여 테스트
  - `npx uri-scheme open “sheme://path”  —ios`
  - `npx uri-scheme open “sheme://path”  —android`
