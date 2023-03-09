---
title: patch-package
author: delaying
date: 2023-3-8 14:55:00 +0900
categories: [Develop, React-Native]
tags: [React-Native, Front-End]
published: true
---

## patch package

라이브러리나 프레임워크 등 npm 패키지 자체의 수정이 필요할 때 [patch-package](https://www.npmjs.com/package/patch-package)라는 패키지를 이용할 수 있다.

패키지 의존성은 그대로 유지하면서, 변경한 npm 패키지의 내용을 버전 관리 대상으로 간단하게 만들 수 있다.

patch-package는 node_modules 안의 수정사항을 Git으로 관리한다.

node_modules 내부의 package 수정사항을 patch 파일 형태로 patches 폴더에 저장하고,
patches 안의 patch를 node_modules 에 적용한다.

## npm 패키지 수정 방법

- github에 fork를 통해 내소유의 repo로 전환 → 이후 잘못된 부분에 대해 commit으로 수정
  - 이후 원래 repo에서 해당 내용을 patch 하게 되는 경우, 기존파일 보존 불가
- patch-package 활용
  - npm install 이후 스크립트를 사용하여 버전을 비교한 뒤 지정한 패치 내용이 적용 되는 것

## 사용방법

`npm install patch-package` 설치 후, package.json의 scripts부분에서 “postinstall”:”patch-package”를 선언해준다.

라이브러리나 패키지들을 설치하기 위해 `npm install`을 하면, 이후 patch package가 자동으로 실행된다.

`npx patch-package [library이름]`시, local 패키지와 npm패키지를 비교한 파일이 생성된다.

## 참고

- [patch-package: package를 빠르고 안전하게 수정하자](https://choyongjoon.com/patch-package/)
- [patch-package를 활용한 NPM 패키지 패치(patch) 사례](https://medium.com/naver-place-dev/patch-package%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-npm-%ED%8C%A8%ED%82%A4%EC%A7%80-%ED%8C%A8%EC%B9%98-patch-%EC%82%AC%EB%A1%80-feat-react-native-ee1fc399b7c2)
