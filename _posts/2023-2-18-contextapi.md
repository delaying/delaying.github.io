---
title: Context API - 전역변수 관리 도구
author: delaying
date: 2023-2-18 8:24:00 +0900
categories: [Develop, React-Native]
tags: [React-Native, React]
published: true
---

Context API는 react 16.3버전부터 지원하고 있다.

props-drilling을 제거하기 위해 도입되었다.

간단한 전역변수를 사용하거나, 간단한 상태관리가 필요할 때 많이 사용한다.

## 구성요소

- Provider : 값을 제공해 주기 위해 root component로 사용한다.
- Consumer : 제공된 값에 접근할 수 있도록 한다.

## Redux와의 차이점

Context API는 상태 관리 도구가 아닌, `전역 변수 관리 도구`이다.

그러므로 redux의 비교대상이 아니라고 할 수 있다.

- 상태관리도구의 조건
  - 초기값을 저장하는가? : context API는 Provider에서 value등을 설정할 수 있음
  - 스스로 값을 읽어올 수 있는가? : context API는 스스로 state를 가지고 있지 않으므로 값을 전달해줘야함
  - 스스로 값 업데이트가 가능한가? : context API는 스스로 state를 가지고 있지 않아 update함수를 함께 전달해줘야함

## 언제 사용할까

주로 static한 변경이 되지 않는 정보에 대해 적용할 때 사용된다.

다크모드 등의 app theme를 저장할 때 사용할 수 있다.

## 사용

app.js에서 외부에 전역값을 선언하고 `createContext()`를 사용한다.

`useState()`로 관리할 값을 작성하여, 전역 컴포넌트로 값을 전달한다.

```
import {createContext} from 'react';
import {CounterScreen} from './src/screens/CounterScreen';

export const CounterContext = createContext();


export default function App(){
	const counterState = useState(0);
	return(
		<SafeAreaProvider>
			<CounterContext.Provider value={counterState}>
				<CounterScreen/>
			</CounterContext.Provider >
		</SafeAreaProvider>
	)
}
```

값이 필요한 곳에서 `useContext()`를 사용하여 전역변수를 사용한다.

```
//CounterScreen.js

import {useContext} from 'react';
import {CounterContext} from '../../App';

const CounterTitle =() => {
	const [count] = useContext(CounterContext);
	return(
		<View> {`${count}개`}</View>
	)
}

export default() => {
	const [count,setCount] = useContext(CounterContext);

	const onPressMinus = useCallback(() => {
		setCount((value) => value-1);
	},[]);

	return(
		<View>
		    <CounterTitle/>
		    {count}
		</View>
	)
}
```
