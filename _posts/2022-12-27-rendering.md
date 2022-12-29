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


## 3. Layout(Reflow)
뷰포트 내에서 생성된 render tree의 노드들이 position등의 style레이아웃이 적용되어 화면공간의 어느 위치에 들어갈지 정해진다

#### Reflow
각요소들의 크기와 위치를 다시 계산해주는 과정을 `Reflow`라고 한다.

Reflow가 일어나는 경우
- 페이지 최초 렌더링 
- 브라우저 viewport 크기 변경
- 폰트 변경
- 이미지 크기 변경
- 요소 위치/크기 변경
- 노드 추가/제거



## 4. Paint(repaint) & 렌더링 최적화
이러한 과정을 통해 화면 UI가 그려지게된다.

그러나 이벤트가 발생하거나 액션이 발생할 때 HTML요소의 스타일에 변화가 생기면 그에 영향받는 노드들의 3번 Layout설정 과정을 다시 수행하게 된다.

#### Repaint
Reflow과정으로 변동된 사항은 Repaint과정을 거치고 나서 실제 화면에 반영이 된다.

레이아웃에 영향을 주지않는 `background-color`, `opacity`같은 스타일 속성이 변경되었을 때는 reflow를 수행하지 않고 repaint과정만 수행된다.



## 렌더링 최적화 방법

1. 폰트용량 줄이기
1. Reflow 최소화하기
2. 독립적인 노드 사용하기


#### 참고
- [`브라우저 렌더링 과정과 최적화`](https://velog.io/@bumsu0211/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95%EA%B3%BC-%EC%B5%9C%EC%A0%81%ED%99%94)
- [`브라우저 렌더링 과정과 최적화(Browser Rendering and Optimization)`](https://velog.io/@ye-ji/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95%EA%B3%BC-%EC%B5%9C%EC%A0%81%ED%99%94Browser-Rendering-and-Optimization)