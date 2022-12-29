---
title: 브라우저 렌더링 과정 
author: delaying
date: 2022-12-27 23:01:00 +0900
categories: [Study, Front-End]
tags: [Front-End, CS]
published: true
---

주소창에 url 주소를 입력했을 때 브라우저에 그려지는 과정에 대해 정리해보려한다

## 1. DOM Tree 생성
브라우저의 렌더링 엔진이 html 코드를 읽고 파싱하여 DOM Tree를 생성한다.


## 2. DOM Tree + CSSOM
HTML 인라인으로 작성된 스타일 코드와 CSS파일을 파싱하여 CSSDOM을 생성하게되고,
이렇게 만들어진 DOM tree와 CSS DOM을 결합하여 렌더트리를 생성하게 된다.


## 3. Layout
뷰포트 내에서 생성된 render tree의 노드들이 position등의 style레이아웃이 적용되어 화면공간의 어느 위치에 들어갈지 정해진다


## 4. Paint
이러한 과정을 통해 화면 UI가 그려지게된다.
