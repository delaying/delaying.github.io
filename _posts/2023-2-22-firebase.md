---
title: React Native에서 Firebase사용하기
author: delaying
date: 2023-2-22 14:23:00 +0900
categories: [Develop, React-Native]
tags: [React-Native]
published: true
---

Firebase는 구글에서 만들어진 BAAS(Backend As A Service)이다.

모바일에서 필요한 저장장치, 테스팅 푸시 등 거의 모든 기능을 제공한다.

- RealTime Database
  - 실시간으로 접근할 수 있는 database
  - NoSQL의 형태로 구성되어있음
  - 실질적으로 저장되는 값은 key와 value값으로 구성된 JSON Object임
  - 실시간으로 저장가능, 오프라인으로도 미리캐싱된 데이터를 기준으로 접근가능한 기능제공, 클라이언트장치에서도 바로엑세스가능
  - 용량 또는 데이터 크기에 과금
  - 제한사항
    - 동시 연결수에 대한 제한이 있음(무료 plan100, 유료 plan20만)
    - 한번에 write은 1MB내외
- Storage
  - 파일저장을 위해서 사용
  - 프로필사진, 임시파일 을 저장하는 기능으로 활용가능함
- Cloud Firestore
  - 데이터를 저장하기위한 제품
  - 실시간성, NoSQL을 지원한다는점은 Realtime database와 비슷
  - 저장하는값 : JSON형태가 아닌 Collections를 저장
  - Document: Data가 집합해있는 단위
  - Collections : Document가 집합해있는 단위
  - document CRUD 횟수에 따라 과금

큰단위 데이터 요청시에는 cloud firestore, 데이터가 작고 crud가 자주 발생하면 realtime database 사용이 유리함

- crashlytics
  - 앱이 강제종료 되었음을 알려주는 Tool
  - 로직을 잘 작성하더라도 라이브러리 등에서 크래시가 날 수도 있음
  - 앱이 정상적으로 배포되었는지 모니터링 가능
- remote config
  - 원격에 있는 상수값을 업데이트 해줄 수 있는 tool
  - 원격으로 특정 기능에 대한 on/off 또는 특정화면의 테스트를 바꾸는것으로 활용
  - 주의점
    - remote config값을 조회 실패했을 때 대비하여 기본값을 설정
    - 실패 등 여러가지 이유로 인하여 최신값을 항상 보여주지는 않음
- AB Test

  - A그룹과 B그룹을 두고 어떤 그룹이 더 많은 전환율을 보이는지 체크
  - 기존버전과 신규로 변경된 버전에서의 분기
  - 개선된 버전에서의 유저 피드백을 받는다는 이점이 있음
  - remote config에서 목표수치를 설정하면 ab test에서 계산해줌
  - 사전작업
    - remote config : config설정된 값들에 대하여 가능
    - analytics : 분석을 위해 필요

- 사용하기

  - react-native-firebase : firebase를 사용가능하도록 만들어둔 package
  - firebase에서 새 프로젝트 생성 후, ios앱과 android를 추가
    - ios : info.plist파일을 다운로드
    - 안드로이드 : google.servies.json 파일 다운로드
  - npx expo install expo-dev-client
  - npx expo install @reaact-native-firebase/app
  - 다운받은 파일들을 프로젝트에 넣기
  - npx expo install expo-build-properties
  - app.json파일에 다음내용 추가 작성

    ```
    'ios':{
    	'googleServicesFile':'./GoogleService-Info.plist',
    },
    'android':{
    	'googleServicesFile':'./google-services.json',
    },
    'plugins':{
    	'@react-native-firebase/app',
    	'@react-native-firebase/perf',
    	'@react-native-firebase/crashlytics',
    	[
    	'expo-build-properties',
    		{
    			'ios':{
    				'useFrameworks':'static'
    			}
    		}
    	]
    }

    ```

  - npx expo prebuild —clean으로 실행 android 와 ios패키지 등록시 사용한 주소 입력

- authentication 적용

  - Social Login
    - 소설계정을 활용하여 로그인 혹은 회원가입을 할 수 있는 기능
    - (google로 로그인, github으로 로그인 등)
    - OAuth2.0
      - 인증을위한 개방형 표준 프로토콜
      - third party 프로그램에게 리소스 소유자를 대신하여 서버에서 데이터제공
      - Authentication : 인증하는단계
      - Authorization : 인증이 끝난 뒤 Access Token이 부여되는 것
      - Access Token : 유저에게 권한 받았음을 인증하는 Token
      - Refresh Token : Access Token을 Refresh하기위한 Token
      - social loginflow
        - 클라이언트에서 social backend로 로그인요청을 보내고 access token을 발급받으면
        - 클라이언트는 backend로 가입요청을 access token과 함께 전달함.
        - backend는 social backend로 데이터를 요청하고, 유저 정보를 전달받음
        - backend에서 client로 소셜 로그인 결과를 전달!
  - firebase authentication의 로그인 제공업체에서 원하는 업체 선택 후 사용설정을 on으로 바꾼 후 저장
  - 최신구성 파일을 다운로드 받음 → ios와 android각각!
  - 파일들을 프로젝트로 옮긴후
  - npm install @react-native-firebase/auth
  - [여기](https://rnfirebase.io/)에서 자세하게 확인가능하다.

  - npx expo prebuild
  - [google-signin](https://github.com/react-native-google-signin/google-signin) 사용

    - npx expo install @react-native-google-signin/google-signin
    - app.json파일 추가작성

    ```
    'plugin':[
    	'@react-native-firebase/app',
    	'@react-native-firebase/auth',
    	'@react-native-google-signin/google-signin',
    ```

    - npx expo prebuild
    - npm start로 정상적으로 작동하는지 확인
    - import한 후 호출

    ```
    import {
      GoogleSignin,
      GoogleSigninButton,
      statusCodes,
    } from '@react-native-google-signin/google-signin';

    GoogleSignin.configure();
    ```

  - 사용하기 위한 코드 작성

  ```

  import { GoogleSignin, GoogleSigninButton, statusCodes } from '@react-native-google-signin/google-signin';
  import { useCallback, useEffect, useState } from 'react';
  import firebaseAuth from '@react-native-firebase/auth'
  GoogleSignin.configure({
  webClientId:'id',
  iosClientId:'key',
  scopes: ['profile', 'email']
  })

  const onPressGoogleSignin = useCallback(async()=>{
  // await GoogleSignin.hasPlayServices();
  try{
  const userInfo = await GoogleSignin.signIn();
  setLoadingUser(true);

        console.log(userInfo);
        const { idToken, serverAuthCode } = userInfo;

        const credential = firebaseAuth.GoogleAuthProvider.credential(idToken);

        const result = await firebaseAuth().signInWithCredential(credential);
        console.log('result', result)

        setUser({
          name: result.additionalUserInfo.profile.name,
          profileImage: result.additionalUserInfo.profile.picture,
        })
        setLoadingUser(false);

      }catch(ex){
          setLoadingUser(false);

          if(ex.code === statusCodes.SIGN_IN_CANCELLED){

          } else if(error.code === statusCodes.IN_PROGRESS){

          } else {

          }
      }

  }, [])

  ```

  - signInSilently()를 사용하면 UI를 띄우지 않고 Google Play Service에 로그인을 시도할 수 있다.

  - firebase storage를 활용한 파일업로드
    - 프로젝트의 storage를 설정
    - 프로덕션 or 테스트모드 선택
    - npx expo install @react-native-firebase/storage
    - npx expo install expo-image-picker : 프로필업로드를 위한 사진불러오는기능
    - [imagepicker](https://docs.expo.dev/versions/latest/sdk/imagepicker/)
    - app.json에 다음을 추가

  ```

      "expo": {
        "plugins": [
          [
            "expo-image-picker",
            {
              "photosPermission": "The app accesses your photos to let you share them with your friends."
            }
          ]
        ]
      }

  }

  ```

  - [cloud storage](https://rnfirebase.io/storage/usage)
