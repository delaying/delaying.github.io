---
title: Error - xcrun exited with non-zero code 22
author: delaying
date: 2023-3-25 15:52:00 +0900
categories: [Develop, TrobleShooting]
tags: [React-Native, TrobleShooting]
published: true
---

## 발생 원인

```
//터미널에서 직접입력 : 정상
npx uri-scheme open "mydog://history" --ios

//노션에서 복사한 코드 : 에러
npx uri-scheme open “mydog://history” --ios
```

Notion에서 코드를 그대로 복사하여 실행했더니 큰따옴표의 유니코드 차이로 인해 uri를 인식하지 못하여 실행이 안되는 에러가 발생했다.

## 발생 에러

<img src="https://user-images.githubusercontent.com/72879145/228152806-c12c53da-550c-40a0-b1d3-65e948a63c80.png" width="600" height="200">

```
LoveDog git:(main) ✗ npx uri-scheme open “mydog://main/page” --ios
› iOS: Opening URI "“mydog://main/page”" in simulator
An error was encountered processing the command (domain=NSPOSIXErrorDomain, code=22):
The operation couldn’t be completed. Invalid URL.
Invalid URL.

Aborting run
An unexpected error was encountered. Please report it as a bug:
Error: xcrun exited with non-zero code: 22
```

## 에러 해결과정

큰따옴표 문제인지 모르고 문제를 찾기위해 많은 시간을 허비하였으나, 우연히 큰따옴표 모양이 다르다는 것을 알게되었다.

코드를 복사하여 사용하지 않고, 터미널에서 키보드로 큰따옴표를 직접 입력하면 해결된다.

## 결과

이번 에러를 통해 같은 문자라도 유니코드가 다를 수 있다는 것을 알게되었다.

노션에서 사용하는 큰따옴표의 유니코드는 201C와 201D를 사용하고, 일반적인 큰따옴표의 유니코드는 22를 사용한다.

**프로그래밍 언어나 쉘에서는 201C와 201D를 큰따옴표로 인정해주지 않아서 발생한 에러였다.**

Notion의 `/code`섹션에서는 큰따옴표 등의 문자들을 다른 유니코드로 변환시키지 않는다고 한다.

다음은 스칼라에서 큰따옴표를 코드로 확인한 유니코드이다.

```
".toHexString //22
“.toHexString //201c
```
