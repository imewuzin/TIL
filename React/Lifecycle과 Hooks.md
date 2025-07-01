# Lifecycle과 Hooks

- Hooks는 왜 등장했을까?
  
  - 초기에는 클래스형 컴포넌트가 기본이었음
  - 하지만 클래스형 컴포넌트의 몇 가지 문제로 인해 새로운 방식 고안 (이해하기 어려움, 코드 재활용성 떨어짐 등)
  - 지금은 Hooks가 완전히 정착하여 기본형으로 사용됨(클래스형은 레거시로 볼 수 있음)

- ### Hooks
  
  - `useState`
  
  - `useEffect`
    
    : React component가 렌더링될 때마다 특정 작업(side effect)을 실행할 수 있도록 하는 리액트 Hook
    
    - Side effect : component가 렌더링된 이후에 비동기로 처리되어야하는 부수적인 효과들
    
    - component가 mount, unmount, update 됐을 때 특정 작업 처리 가능
    
    - 사용 방법
      
      - 불러오기 : `import React, { useEffect } from 'react';`
      
      - `useEffect ( function, deps)`
        
        - `function` : 수행하고자 하는 작업 (리액트는 이 함수를 기억했다가 DOM 업데이트 후 불러냄). 또한, 이 함수에서 함수를 `return`할 경우 그 함수가 component가 unmount될 때 다시 한번 실행
        
        - `deps` 배열 형태. 배열 안에는 검사하고자하는 특정 값 or 빈 배열. deps에 특정 값을 넣게 되면 component가 mount될 때, 지정한 값이 업데이트될 때 `useEffect` 실행
      
      - 사용 방식
        
        - component가 mount됐을 때 (처음 나타났을 때)
          
          - 컴포넌트가 화면에 가장 처음 렌더링될 때 한번만 실행하고 싶다면 `deps` 위치에 빈 배열
          
          > ```jsx
          > useEffect(() => {
          >     console.log('마운트 될 대만 실행');
          > }, []);
          > ```
          > 
          > - 배열 생략하면 리렌더링될 때마다 실행됨
        
        - component가 update될 때 (특정 props, state가 바뀔 때)
          
          - `deps` 위치의 배열 안에 검사하고 싶은 값 넣기
          
          > ```jsx
          > useEffect(() => {
          >     console.log(name);
          >     console.log('업데이트 될 때 실행;);
          > }, [name]);
          > ```
          > 
          > - 업데이트 될 때만 실행시키고 싶다면 아래와 같이
          >   
          >   ```jsx
          >   const mounted = useRef(false);
          >   
          >   useEffect(() => {
          >   
          >     if (!mounted.current) {
          >     mounted.current = true;
          >     } else {
          >         console.log(name);
          >         console.log('업데이트 될 때 실행');
          >     }
          >   
          >    }, [name]);
          >   ```
        
        - component가 unmount 될 때 (사라질 때) & update 되기 직전
          
          - `cleanup` 함수 반환 (`return` 뒤에 나오는 함수. `useEffect`에 대한 뒷정리 함수라 함)
          
          - unmount될 때만 `cleanup` 함수를 실행하고 싶다면 `deps`에 빈 배열
          
          - 특정 값이 업데이트 되기 직전에 `cleanup`함수를 실행하고 싶다면, `deps` 배열 안에 검사하고 싶은 값을 넣어줌
            
            - `useEffect` 함수가 실행되기 전에 (처음 실행되는 경우를 제외하고는) `cleanup` 함수가 실행됨. 또한, `cleanup` 함수는 DOM에서 마운트 해제될 때마다 실행됨
            - `cleanup` 함수는 component가 재사용 될 때마다 실행되며 모든 새로운 `sideEffect` 함수가 실행되기 전에 (처음 실행 제외), component가 제거되기 전에 실행됨
          
          > ```jsx
          > useEffect(() => {
          >     console.log('effect');
          >     console.log(name);
          >     return () => {
          >         console.log('cleanup');
          >         console.log(name);
          >     };
          > }, []);
          > ```
  
  - `useCallback`
  
  - `useMemo`, `useContext`, `useRef`, `useLayoutEffect`
    
    > ```jsx
    > # components/Counter.js
    > import { useCallback, useEffect, useState } from "react";
    > 
    > function Counter() {
    >   console.log("Render Counter!");
    > 
    >   /**
    >    * useState: 상태 값과 그 값을 갱신하는 함수를 반환
    >    * - 인자: 초기 상태 값
    >    * - 반환: [상태 변수, 상태에 대한 Setter]
    >    */
    >   const [value, setValue] = useState(0);
    > 
    >   /**
    >    * useEffect: 컴포넌트가 렌더링 될 때, 특정 작업을 실행
    >    * - 인자
    >    *   - 실행하고자 하는 함수 (effect callback)
    >    *     - effect는 정리(clean-up) 함수를 반환할 수 있음
    >    *     - 반환된 함수는 컴포넌트가 언마운트 또는 effect 재실행 이전에 실행됨
    >    *   - 의존성 배열 (dependency list)
    >    */
    >   useEffect(() => {
    >     console.log("[Function] useEffect []: 컴포넌트가 마운트 될 때, 한 번만!");
    > 
    >     return () => {   // clean-up 함수
    >       console.log("[Function] useEffect return []: 컴포넌트가 언마운트 될 때,");
    >     };
    >   }, []);
    > 
    >   // 가장 끝 [value] 값이 변경되는 순간에 useEffect를 실행하겠다.
    >   useEffect(() => {
    >     console.log(
    >       "[Function] useEffect [value]: 컴포넌트가 마운트 될 때, + value가 변경되면,"
    >     );
    > 
    >     return () => {
    >       console.log(
    >         "[Function] useEffect return [value]: 새로 useEffect를 수행하기 전에,"
    >       );
    >     };
    >   }, [value]);
    > 
    >   /**
    >    * useCallback: 메모이제이션된 콜백을 반환
    >    * - 인자
    >    *   - 메모이제이션 할 함수
    >    *   - 의존성 배열
    >    * - 반환: 메모이제이션 된 함수
    >    * *의존성 배열을 제대로 셋팅하지 않으면 함수 안에서 사용되는 값이 업데이트 되지 않은 값일 수 있음
    >    */
    >   // useCallback은 아래 경우보다는 reset 같은 역할하는 함수에 효과
    >   const increaseValue = useCallback(() => {
    >     setValue(value + 1);
    >   }, [value]);
    > 
    >   return (
    >     <div>
    >       <h1>value: {value}</h1>
    >       <button onClick={increaseValue}>Increase value</button>
    >     </div>
    >   );
    > }
    > 
    > export default Counter;
    > ```

---

# React 렌더링 과정

- Rerendering
  
  : 상태(State, Props)가 변경되면 화면을 다시 그림
  
  - class 형 component의 Lifecycle
    
    ![](C:\Users\SSAFY\Desktop\TIL\image\class형component의Lifecycle.png)
    
    - `constructor` 함수 -> `render` 함수 -> `componentDidMount` -> `shouldcomponentUpdate` (`return`값 false면 리랜더링x) ->리랜더링 -> `componentDidupdate` -> 언마운트되면 `componentWillUnmount` 
  
  - 함수형 component의 Lifecycle -> 주로 사용
    
    ![](C:\Users\SSAFY\Desktop\TIL\image\function형component의Lifecycle.png)
    
    - 랜더링되면 `useEffect` 함수 등 함수는 다 무시하고 `return` 속 jsx로 화면 띄움 -> `useEffect` 함수들 실행 -> 리랜더링되면 함수 속 `useEffect` 함수 등 다 무시하고 `return` 속 jsx 화면에 반영 -> `useEffect`의 `cleanup`함수 먼저 실행 -> `useEffect` 함수 실행
