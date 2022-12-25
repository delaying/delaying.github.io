---
title: DOM과 Virtual Dom 
author: delaying
date: 2022-12-24 22:12:00 +0900
categories: [Study, Front-End]
tags: [Front-End,React]
published: true
---

## Dom 이란?
DOM은 문서객체모델(Document Object Model)의 약자로서 HTML 문서를 파싱하여 문서의 구성요소를 구조화한 것이다.

DOM은 문서를 노드와 객체로 나타낸다.

document -> html -> head,body
와 같은 형태의 트리구조로 생성된다.

내가 작성한 코드가 브라우저가 이해할 수 있도록 객체로 변환되어야 하는데 DOM이 그 역할을 해준다.

### DOM 동작 원리
브라우저에서 HTML을 파싱하여 노드들로 이루어진 DOM 트리를 만든다. 그리고 CSS파일의 스타일을 파싱하여 스타일을 입힌 트리를 만들게 된다.

DOM은 내용이 조금만 바뀌어도 처음부터 다시 HTML을 파싱하여 DOM트리를 만들고 CSS파싱하여 Render트리를 만들게 되므로 비효율적이다.

그래서 Virtual DOM 방식이 나오게 되었다.


## Virtual DOM이란?
Virtual DOM은 한번만 렌더링을 일으키며, 이전 상태와 바뀐점을 비교하여 달라진 부분만 DOM으로 전달한다.


### Vitual DOM 사용이유
SPA(Single Page Application) 방식에서 DOM을 직접적으로 계속 재렌더링하면 PC자원을 많이 소모하게 되므로 virtual DOM 방식이 사용된다.

React나 Vue와 같은 프레임워크가 Virtual DOM을 사용한다.

### Virtual DOM 동작원리
DOM을 계속 재렌더링하지 않기 위해 Virtual DOM이라는 가상 DOM을 메모리에 만들어 두게된다.

변경사항이 생기면 Virtual DOM을 수정하고, Virtual DOM을 통해 DOM을 수정한다.

상태가 변한 컴포넌트를 루트노드로 하여 깊이 우선 탐색 방식으로 그부분만 render를 진행한다.
이렇게 얻은 새로운 Virtual DOM을 실제 DOM과 동기화된 기존 Virtual DOM과 비교하여 변경사항을 파악한다.
그리고 실제 변경사항만 DOM API를 호출하여 DOM에 반영하게 된다.



#### 참조

- [`Virtual DOM 동작 원리와 이해 (with 브라우저의 렌더링 과정)`](https://jeong-pro.tistory.com/210)
- [`DOM과 Virtual Dom이란?`](https://www.howdy-mj.me/dom/what-is-dom)
- [`React Virtual DOM 이해하기`](https://velog.io/@gwak2837/React-Virtual-DOM-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)