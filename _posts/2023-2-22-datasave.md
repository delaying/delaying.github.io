---
title: 데이터 저장소 - AsyncStorage, Fetch API, Redux-persist
author: delaying
date: 2023-2-22 13:21:00 +0900
categories: [Develop, React-Native]
tags: [React-Native, React, Front-End]
published: true
---

데이터를 저장하고 가져오는 방법은 매우 다양하다.

그중에서 AsyncStorage, Fetch API, redux-persist에 대해 살펴보려한다.

## AsyncStorage

- key값으로 string을 저장하며, 웹의 cookie와 비슷한 역할
- android: SQLite에 저장
- ios : 네이티브 코드로 저장로직이 작성되어 있음
- AsyncStorage.setItem
  - key값과 value값을 넘겨 값을 저장
  - 저장하는 value는 string으로만 저장할 수 있음
    객체라면 json.stringify로 변환해서 저장 해야 함
- AsyncStorage.getItem
  - 값을 가져옴
  - string 또는 null을 리턴하므로 string으로 변환한 객체라면 json.parse로 다시 object로 바꿔 사용해야함
- AsyncStorage.removeItem
  - key값에 해당하는 value를 삭제
- AsyncStorage.clear
  - AsyncStorage의 모든 값을 삭제
- AsyncStorage.mergeItem
  - object안에서 같은 key값이 있는 경우 한가지로 합침
- 그외

  - multiGet,multiSet,multiMerge,multiRemove
  - 각각 key값을 여러개 넘겨 한번에 받아오는것

- 사용시 주의점
  - key값 중복으로 인한 value 덮어써짐이나 삭제될 수 있음
    이를 해결하기 위해 Unique한 문자열을 만들기 위해 UUID문자열을 만들어 사용하거나 화면 또는 동작을 String으로 조합하여 key값으로 사용함
  - AsyncStorage의 모든 함수는 Promise로 제공됨
  - Android에서 최대 저장 사이즈는 6MB, android에서 한번에 가져올 수 있는 사이즈는 2MB.

## fetch API

- Fetch API
  - RemoteURL에 있는 리소스를 가져올때 사용
  - HTTP 통신도구
- HTTP
  - Hyper Text Transfer Protocol의 약자
  - server에 데이터를 저장,업데이트 등을 요청하고 결과를 되받는것
  - Request Method
    - 어떤 동작을 위한 요청인지를 미리 나타내는 것
    - 종류 : get,post,put,patch,delete
    - get : 특정 리소스를 가져와야할 때 사용 ex) 특정회원데이터조회
    - post :어떤 데이터를 저장할 때 사용 ex)회원가입, 로그인
    - put : 특정 리소스를 업데이트 할 때 사용 ex)회원주소데이터,결제데이터
    - patch : 특정 리소스 중 특정 정보만 변경할 때 사용 ex)회원나이,전화번호
    - delete : 리소스 삭제 ex)회원탈퇴
  - request 데이터 전달
    - 데이터를 서버로 전달하는 방법
    - path parameter, query parameter, request body 등
    - path parameter : URL path내부에 값을 함께 넘기는 것 ex) /user/{:userId}/
    - Query parameter : URL뒷부분에 ?를 붙이고 그뒤에 key값과 value값을 넘겨주는것 ex) /user?birthday={:date}&sort={:regeditDate}
    - Request Body : URL에 데이터가 보이지않고, Body에 작성해서 넘기게됨, 데이터가 긴 경우 Request body사용이 적합.(URL은 길이제한있음)
  - request 데이터 전달 사용처 예시
    - path parameter : 특정 유저 ID를 통해 정보를가져올 때
    - query parameter : 이름,성별로 검색해서 가져올때
    - request body : 회원가입, 게시글 등록할때
  - response status code
    - 결과 처리에 대해서 숫자 형태로 서버로부터 전달받음
    - 자주사용하는 코드 [200,400,403,404,500]
    - 200 : 정상적으로 처리됨
    - 500 : 서버에서 처리 중 에러가 발생함
    - 400 : client에서 값을 잘못 전달함
    - 403 : 유저정보는 식별되나, 해당 URL로의 접근이 거부됨
    - 404 : URL이 존재하지않음
- Fetch API 사용방법
  ```
  fetch(REQUEST_URL, { method: POST, body: {} }).then((result) =>
    result.json()
  );
  ```

## redux-persist

- Redux-persist
  - 저장소에 마지막 Redux상태를 저장하였다가 이어서 사용할 수 있도록하는것
  - React-Native에서는 AsyncStorage에 저장
- PersistGate
  - Component형태로 작성되어있음
  - Storage로부터 데이터를 로드해 Redux를 업데이트
  - 로딩하는 동안 Loading 컴포넌트 추가 가능
- BlackList, WhiteList
  - BlackList : 유지하지 않아도 되는 Redux Key값들
  - WhiteList : 유지를 해야하는 key값
- 적용
  - npm install redux-persist —save
  - npm install @react-native-async-storage/async-storage —save
