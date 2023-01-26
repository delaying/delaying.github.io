---
title: React 클래스형과 함수형 컴포넌트
author: delaying
date: 2023-1-24 23:01:00 +0900
categories: [Study, Front-End]
tags: [Front-End, React]
published: true
---

React 버전 16.8 부터 Hook이 추가되었다.<br/>
원래는 클래스형 컴포넌트를 많이 사용하였으나, 함수형 컴포넌트에서도 state를 관리할 수 있게되어 React 공식문서에서도 함수형 컴포넌트 사용을 추천하고 있다.


현재 클래스형 컴포넌트를 자주 사용하진 않지만, 이전까지의 코드들에서는 많이 사용되므로 개념은 알고 있어야 한다.



## class와 function형 차이
### 선언방식
- 클래스형 컴포넌트
    ```
    import React, {Component} from 'react';

    class App extends Component {
        render() {
            const str = 'abc';
            return <div>{str}</div>
        }
    }

    export default App;
    ```
    - class 키워드를 작성해야함
    - Component 상속을 받아야함
    - render() 메소드가 있어야함


- 함수형 컴포넌트
    ```
    import React from 'react';

    function App() {
        const str = 'abc';
        return <div>{str}</div>
    }

    export default App;
    ```

### state 사용방법
- 클래스형 컴포넌트
    ```
    class App extends Component {
        constructor(props){
            super(props);

            this.state = {
                load : false,
            }
        }
        render() {
            const str = 'abc';
            return <div onClick={()=>{this.setState({load:true});}}>{str}</div>
        }
    }
    ```
    Constructor 안에서 this.state에서 초기값을 설정 할 수 있다.

    그리고 state값을 변경할 때는 this.setState 함수를 사용해야 한다.


- 함수형 컴포넌트
    ```
    import React, { useState, useEffect } from 'react';

    function Example() {
        const [load, setLoad] = useState(false);
        const str = 'abc';

        useEffect(() => {
            str = str + 'de';
        },[load]);

        return (
            <div onClick={() => setLoad(true)}>
                {str}
            </div>
        );
    }
    ```
    useState Hook 함수를 사용하여 state를 관리한다.

    또한 useEffect Hook을 이용하여 컴포넌트가 렌더링 이후 어떤일을 수행할 지 작성할 수 있다.<br/>
    클래스의 componentDidMount와 componentDidUpdate, componentWillUnmount가 합쳐진 것과 비슷하다.


## 클래스형 선언방식의 단점
`componentDidMount`, `componentWillUnmount` 등과 같은 상태 관련로직들은 이해가 어렵고, 함께 사용하지만 연관없는 코드들은 무결성을 해치기 쉽다.

상태 관련 로직은 한 공간안에 묶여 있기 때문에 컴포넌트들을 작게 분리하는 것이 불가능하고, 테스트하기에도 어려움이 있다.

또한, `this`키워드를 많이 사용하는데 이 키워드가 어떻게 작동하는지에 대한 이해가 필요하며, 코드의 재사용성과 구성을 어렵게 만든다.

Class는 최근 사용되는 도구에서도 많은 문제를 일으킨다.



이처럼 공식문서에서 Class보다는 function형과 hook을 같이 사용하는게 더 바람직하다고 말하고 있다.


#### 참고
- [`React-Hook의 개요`](https://ko.reactjs.org/docs/hooks-intro.html)
- [`클래스형과 함수형 차이`](https://velog.io/@sdc337dc/0.%ED%81%B4%EB%9E%98%EC%8A%A4%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)