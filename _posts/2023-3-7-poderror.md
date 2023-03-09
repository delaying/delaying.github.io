---
title: MacOS 업데이트 이후, pod 명령어 인식오류
author: delaying
date: 2023-3-7 14:31:00 +0900
categories: [Develop, TrobleShooting]
tags: [React-Native, TrobleShooting]
published: true
---

## 발생 원인

MacOS 소프트웨어 업데이트 이후로 pod명령어가 인식되지 않는 문제가 발생했다.

## 해결 과정

1. cocoapods 재설치
   `sudo gem install cocoapods`로 진행하였으나, 다음과 같은 에러가 출력되었다.
   `You don t have write permissions for the Library Ruby Gems 2.6 0 directory cocoapods`

2. ruby 버전은 2.7.0이상 사용하라고 힌트를 줘서 global로 루비 버전을 설정했어도 같은 에러가 발생했다.
   `rbenv global 2.7.7`
3. rbenv global 명령을 실행 후에도 ruby 버전이 바뀌지 않아 이 [사이트](https://codecamper.me/blog/122/)를 참고하여 해결하였다.

   나는 `rbenv versions`과 `ruby --version`으로 확인한 버전이 다른 경우에 해당했다.

   `rbenv init` 을 실행하고, 출력되는 eval~ 구문을 ~/.zshrc파일에 작성하니 pod 명령어가 정상적으로 인식되었다.
