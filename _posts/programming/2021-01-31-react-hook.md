---
title:  "[React] React Hook"
excerpt: ""

categories:
  - programming
tags:
  - [react, hook]

toc: true
toc_sticky: true
 
date: 2021-01-31
last_modified_at: 2021-01-31
---

# Hook

React 16.8 부터 Hook이 추가 되어 Class 없이 상태 값과 React의 기능들을 사용 가능하다.

> Hook은 Class안에서 동작하지 않는다.

## Hook 종류
- 기본 Hook
    - [useState](#state-hook)
    - [useEffect](#effect-hook)
    - useContext

- 추가 Hooks
    - useReducer
    - useCallback
    - useMemo
    - useRef
    - useImperativeHandle
    - useLayoutEffect
    - useDebugValue

---

# Hooks 정리

## State Hook

Class를 사용할 때 아래의 예시와 같이 state, setState를 사용하는 것을 대체 할 수 있다. Class에서와 달리 변수 각각 useState를 이용해 변수를 업데이트 해야한다.

- 예시

``` javascript
import React, { useState } from 'react'; // react package로 부터 useState를 import해서 사용한다.

function Example() {
  const [age, setAge] = useState(0); // value, value setter를 useState()로 부터 리턴 받아 사용한다. 인자로 default 값을 넣어준다.

  return (
    <div>
      <p>My age : {age}</p> {/* useState 로 부터 리턴받은 age 값을 사용할 수 있다.*/}
      <button onClick={() => setAge(age + 1)}> {/* useState 로 부터 리턴받은 age setter 함수를 사용해 값을 업데이트 할 수 있다..*/}
        Click to increase age.
      </button>
    </div>
  );
}
```

## Effect Hook

Class의 componentDidMount와 componentDidUpdate, componentWillUnmount 처럼 Rendering 후 또는 변수 업데이트 시, 원하는 side effect들을 수행할 수 있다.

- 예시 - 첫번째 렌더링, 이후 업데이트 시 수행 (DOM이 업데이트 된 후!)

``` javascript
import React, { useState, useEffect } from 'react'; // react package로 부터 useEffect를 import해서 사용한다.

function Example() {
  const [age, setAge] = useState(0);

  useEffect(() => { // Rendering 후에 수행할 effect를 설정할 수 있다.
      document.title = `My Current Age: ${age}`;
  });

  return (
    <div>
      <p>My age : {age}</p>
      <button onClick={() => setAge(age + 1)}>
        Click to increase age.
      </button>
    </div>
  );
}
```

- 예시 - 첫번째 렌더링, 이후 특정 값 업데이트 될 때만 수행 (DOM이 업데이트 된 후!)

``` javascript
import React, { useState, useEffect } from 'react'; // react package로 부터 useEffect를 import해서 사용한다.

function Example() {
  const [age, setAge] = useState(0);

  useEffect(() => {
      document.title = `My Current Age: ${age}`;
  }, [age]); // 최초 Rendering 후 혹은 두번째 인자([age])의 값 업데이트 시에 수행할 effect를 설정할 수 있다.

  return (
    <div>
      <p>My age : {age}</p>
      <button onClick={() => setAge(age + 1)}>
        Click to increase age.
      </button>
    </div>
  );
}
```


- 예시 - Clean Up의 활용 - useEffect에서 함수를 return
    - componentWillUnmount의 경우와 비슷
    - 바로 실행되지 않고, 다음 Effect 실행 전에 return 되었던 함수가 실행된다.

``` javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [age, setAge] = useState(0);

  useEffect(() => {
      document.title = `My Current Age: ${age}`;

      return () => console.log("Test Log"); // 함수를 리턴할 경우 -> 바로 실행되지 않고, 다음 Effect 실행 전에 수행
  });

  return (
    <div>
      <p>My age : {age}</p>
      <button onClick={() => setAge(age + 1)}>
        Click to increase age.
      </button>
    </div>
  );
}
```

---

## Source
- https://ko.reactjs.org/docs/hooks-intro.html