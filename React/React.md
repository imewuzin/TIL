# React

## React 시작하기

1. Node.js, npm, VS Code 설치

2. `$ npx create-react-app 프로젝트이름`

3. 프로젝트로 이동 `cd 프로젝트이름`

4. 실행 `npm start`

---

## JSX

: JavaScript의 확장 문법. JavaScript + XML/HTML

- ex) `const element = <h1>Hello, world!</h1>;`, `<div>Hello, {name}</div>`

- 역할 : XML/HTML 코드를 JavaScript로 변환

- 장점
  
  - 코드가 간결해짐
  
  - 가독성이 올라가 버그 발견이 쉬움
  
  - Injection Attacks 방어

- 사용법
  
  - JavaScript 코드는 `{}` 사용
  
  - XML/HTML 코드들 사이에 JavaScript 사용

---

## Rendering Elements

: 어떤 물체를 구성하는 성분. 리액트 앱을 구성하는 가장 작은 블록들

- DOM Element는 많은 정보를 담고 있기 때문에 React Element보다 무거운

- React Element는 JavaScript 객체 형태로 존재

- `createElement`함수
  
  : component 랜더링을 위해 모든 component가 `createElement` 함수를 통해 element로 변환됨
  
  > ```jsx
  > React.createElement(
  >     type,
  >     [props],  // 속성
  >     [...children]  // 해당 element의 자식 element
  > )
  > ```

- 특징
  
  - 불변성 : Elements 생성 후에는 children이나 attributes를 바꿀 수 없음
    
    - component는 붕어빵 틀, element는 붕어빵이라 생각 (props는 재료)
    
    - 변경된 element를 보여주기 위해서는 새로운 element를 만들어야 함

- Elements 랜더링하기
  
  - Root DOM Node : 최상단 노드
    
    - `<div id="root"></div>`
  
  - Virtual DOM에서 실제 DOM으로 이동하는 과정
  
  - 예시 - element 하나 생성 후, root DIV에 랜더링
    
    > ```jsx
    > const element = <h1>안녕, 리액트!</h1>
    > ReactDOM.render(element, document.getElementById('root'));
    > ```
    > 
    > - 첫번째 파라미터인 `element`를 두번째 파라미터인 `document.getElementById('root')`(DOM Element)에 랜더링
    > 
    > - React Element는 리액트의 Virtual DOM에 존재, DOM Element는 실제 브라우저의 DOM에 존재

---

## Components

- React는 Component 기반. 작은 컴포넌트들이 모여 하나의 컴포넌트를 구성하고, 화면을 구성함

- 입력 (`Props`) -> 함수 (`React Component`) -> 출력 (`React Element`)
  
  - 리액트 `Component`는 만들고자 하는 대로 `props`(속성)을 넣으면 해당 속성에 맞춰 홤녀에 나타날 `element`를 만들어줌

- Pure 함수 역할 : 모든 리액트 컴포넌트는 Props를 직접 바꿀 수 없고, 같은 Props에 대해서는 항상 같은 결과를 보여줘야 함

- 종류
  
  - ### Function Component
    
    - 예시
      
      > ```jsx
      > function Welcome(props) {
      >     return <h1>안녕 {props.name}</h1>;
      > }
      > ```
      > 
      > - 하나의 props 객체를 받아 인삿말이 담긴 React 엘리먼트 반환하는 React 함수 컴포넌트
  
  - ### Class Component
    
    : JavaScript ES6의 Class를 사용해 만들어진 컴포넌트
    
    - 리액트의 모든 Class Component는 `React.Component`를 상속받아 만들어짐
    
    - 예시
      
      > ```jsx
      > class Welcome extends React.Component{
      >       render(){
      >         return <h1>안녕 {props.name}</h1>;
      >       }
      > }
      > ```
      > 
      > - 하나의 props 객체를 받아 인삿말이 담긴 React 엘리먼트 반환하는 React 함수 컴포넌트

- 이름
  
  - 항상 대문자로 시작해야 함
  - 소문자로 시작하면 DOM 태그로 인식해버림

- 렌더링
  
  > - welcome 컴포넌트에 name props 넣어 렌더링하기
  >   
  >   ```jsx
  >   function Welcome(props) {
  >     return <h1>안녕, {props.name}</h1>;
  >   }
  >   
  >   const element = Welcome name="유진" />;
  >   
  >   ReactDom.render(
  >     element,
  >     document.getElementById('root')
  >   );
  >   ```

- Component 합성
  
  : 복잡한 화면을 여러 개의 Component로 나눠서 구현 가능

- Component 추출
  
  > ```jsx
  > function Comment(props) {
  >     return (
  >         <div className="comment">
  >             <div className="user-info">
  >                 <img className="avatar"
  >                     src={props.author.avatarUrl}
  >                     alt={props.author.name}
  >                 />
  >                 <div className="user-info-name">
  >                     {props.author.name}
  >                 </div>
  >             </div>
  > 
  >             <div className="comment-text">
  >                 {props.text}
  >             </div>
  > 
  >             <div className="comment-date">
  >                 {formatDate(props.date)}
  >             </div>
  >         </div>
  >     );
  > }
  > ```
  > 
  > ```jsx
  > props = {
  >     author: {
  >         name: "유진",
  >         avatarUrl: "https://...",
  >     },
  >     text: "댓글입니다.",
  >     date: Date.now(),
  > }
  > ```
  > 
  > 1. Avatar 추출
  >    
  >    ```jsx
  >    function Avatar(props) {
  >        return(
  >            <img className="avatar"
  >            src={props.user.avatarUrl}
  >            alt={props.user.name}
  >            />
  >        );
  >    }
  >    ```
  > 
  > 2. UserInfo 추출
  >    
  >    ```jsx
  >    function UserInfo(props) {
  >        return(
  >            <div clasName="user-info">
  >                <Avatar user={props.user} />
  >                <div className="user-info-name">
  >                    {props.user.name}
  >                </div>
  >            </div>
  >        );
  >    }
  >    ```
  > 
  > 3. 최종 코드
  >    
  >    ```jsx
  >    function Comment(props) {
  >        return(
  >            <div clasName="comment">
  >                <UserInfo user={props.author} />
  >                <div className="comment-text">
  >                    {props.text}
  >                </div>
  >    
  >                <div className="comment-date">
  >                    {formatDate(props.date)}
  >                </div>
  >            </div>
  >        );
  >    }
  >    ```

---

## Props

  : 속성

- 특징
  
  - Read-only : 다른 값을 주려면 새로 값을 전달해 새로운 element를 만들어야 함
  
  - **문자열 이외에 정수, 변수, 다른 컴포넌트가 들어갈 때에는 `{}`로 감싸야 함**

- 사용법
  
  - JSX - 키값의 형태로 props 넣을 수 있음
    
    > ```jsx
    > function App(props) {
    >     return (
    >         <Profile
    >             name="유진"
    >             introduction="안녕하세요! 유진입니다."
    >             viewCount={1500}
    >         />
    >     );
    > }
    > ```
    > 
    > - App 컴포넌트 속 Profile 컴포넌트
    > 
    > - Profile 컴포넌트 속 name, introduction, viewCount 속성(props)
    >   
    >   - 중괄호 o : JavaScript 코드. 문자열 이외에는 중괄호로 감싸야 함

---

## State

: 리액트 Component의 상태(변경 가능한 데이터)

- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 함

- state는 JavaScript의 객체임

> ```jsx
> class LikeButton extends React.Component {
>     constructor(props) { // 생성자
>         super(props);
> 
>         this.state = {  // 현재 state를 정의
>             liked: false
>         };
>     }
>     ...
> }
> ```

- state는 직접 수정할 수 없음 (하면 안 됨)

> ```jsx
> // state를 직접 수정(잘못된 사용법)
> this.state = {
>     name: 'Yujin'
> };
> 
> // setState 함수를 통한 수정(정상적인 사용법)
> this.setState({
>       name: 'Yujin'
> });
> ```

---

## Lifecycle

: 리액트 Component의 생명주기

- 리액트 클래스 컴포넌트의 생명 주기
  
  ![](C:\Users\SSAFY\Desktop\TIL\image\class형component의Lifecycle.png)
  
  1. Mount(생성) - `consntuctor` 생성자 실행 (컴포넌트의 state 정의)
  
  2. `render` 
  
  3. `componentDidMount` 함수 호출
  
  4. Update (변화 겪음. 여러번 rendering됨) - 컴포넌트의 props 변경 or `setState()` or `forceUpdate()`
  
  5. `render`
  
  6. `componentDidUpdate` 함수 호출
  
  7. Unmount(사망) - 상위 컴포넌트에서 현재 컴포넌트를 더이상 화면에 표시X
  
  8. `componentWillUnmount` 함수 호출

- Component가 계속 존재하는 것이 아니라, 시간의 흐름에 따라 생성되고 업데이트 되다가 사라짐

| Function Component     | Class Component                |
| ---------------------- | ------------------------------ |
| state 사용 불가            | 생성자에서 state를 정의                |
| Lifecycle에 따른 기능 구현 불가 | `setState()` 함수를 통해 state 업데이트 |
| Hooks                  | Lifecycle methods 제공           |

---

## Hooks

: React의 state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 실행하도록 만든 state, Lifecycle, 최적화 관련 함수. `use`가  이름 앞에 붙음

- <mark>`useState()`</mark>
  
  : state를 사용하기 위한 Hook
  
  - 사용법 : `const [변수명, set함수명 ] = useState(초기값);`
  
  > ```jsx
  > import React, { useState } from "react";
  > 
  > function Counter(props) {
  >     const [count, setCount] = useState(0);
  > 
  >     return (
  >         <div>
  >             <p>총 {count}번 클릭했습니다.</p>
  >             <button onClick={() => setCount(count+1)}>
  >                 클릭
  >             </button>
  >         </div>
  >     );
  > }
  > ```
  > 
  > - count 값이 변경되면 컴포넌트가 재렌더링 되면서 화면에 새로운 카운트 값 표시
  > 
  > - class 컴포넌트에서 `setState` 함수를 호출해서 state가 업데이트 되고, 이후 컴포넌트가 재렌더링 되는 과정과 동일
  > 
  > - class 컴포넌트에서는 `setState` 함수 하나를 사용해서 모든 state 값을 업데이트 할 수 있었지만, function 컴포넌트의 `useState`를 사용하는 방법에서는 변수 각각에 대해 `set` 함수가 따로 졵

- <mark>`useEffect()`</mark>
  
  : Side effect를 수행하기 위한 Hook
  
  - Side effect : 효과, 영향. 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문에 렌더링이 끝난 이후에 실행되어야 하는 작업들
  
  - class 컴포넌트에서 제공하는 생명주기 함수인 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`와 동일한 기능을 하나로 통합해 제공
  
  - 사용법 : `useEffect(이펙트 함수, 의존성 배열);`
    
    - 컴포넌트 mount 이후, **의존성 배열 안 하나라도 값이 변경되었을 때** 이펙트 함수가 실행됨
    
    - 기본적으로 이펙트 함수는 처음 컴포넌트가 렌더링 된 이후와 업데이트로 인한 재렌더링 이후에 실행됨
    
    - 의존성 배열에 빈 배열`[]` : 이펙트 함수가 mount와 unmount 시 단 한번씩만 실행(props, state 어떤 것에도 의존X. 여러번 실행X)
    
    - 의존성 배열 생략 : 컴포넌트가 업데이트될 때마다 호출됨
    
    - `return` 함수 : `componentWillUnmount` 함수 역할
  
  > ```jsx
  > import React, { useState, useEffect } from "react";
  > 
  > function Counter(props) {
  >     const [count, setCount] = useState(0);
  > 
  >     // componentDidMount, componentDidUpdate와 비슷하게 작동
  >     useEffect(() => {
  >         // 브라우저 API를 사용해 doucument의 title 업데이트
  >         document.title = `You clicked ${count} times`;
  >     });
  > 
  >     return (
  >         <div>
  >             <p>총 {count}번 클릭했습니다.</p>
  >             <button onClick={() => setCount(count+1)}>
  >                 클릭
  >             </button>
  >         </div>
  >     );
  > }
  > ```
  > 
  > - 위처럼 의존성 배열 없이 `useEffect` 사용 시, React는 DOM이 변경된 이후에 해당 이펙트 함수를 실행하라는 의미로 받아들임
  > 
  > - 컴포넌트가 처음 렌더링될 때를 포함해 매번 렘더링될 때마다 이펙트 함수 호출됨
  > 
  > - Effect는 함수 컴포넌트 안에서 선언되었기 때문에 해당 컴포넌트의 props와 state에 접근 가능 -> `count`라는 state에 접근해 해당 값이 포함된 문자열을 생성해 사용함
  > 
  > ```jsx
  > import React, { useState, useEffect } from "react";
  > 
  > function UserStatus(props) {
  >     const [isOnline, setIsOnline] = useState(null);
  > 
  >     function handleStatusChange(status) {
  >         setIsOnline(status.isOnline);
  >     }
  > 
  >     useEffect(() => {
  >         ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
  >         // 컴포넌트가 unmount될 때 구독을 해지하는 api 호출
  >         return () => {
  >             ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
  >         };
  >     });
  > 
  >     if (isOnline === null) {
  >         return '대기 중...';
  >     }
  >     return isOnline ? '온라인' : '오프라인';
  > }
  > ```

- <mark>`useMemo`</mark>
  
  : Memoized value를 리턴하는 Hook
  
  - Memoized value : Memoization된 결과 값
  
  - Memoization : 연산량이 많이 드는 함수의 호출 결과를 저장해 두었다가, 같은 입력 값으로 함수를 호출하면 새로 함수를 호출하지 않고 이전에 저장해놨던 호출 결과를 바로 반환
  
  - 사용법
    
    ```jsx
    const memoizedValue = useMemo(
        () => {
            // 연산량이 높은 작업을 수행하여 결과를 반환
              return computeExpensiveValue(의존성 변수1, 의존성 변수2);
          },
          [의존성 변수1, 의존성 변수2]
    );
    ```
    
    - **의존성 배열에 들어있는 변수가 변했을 경우에만** 새로 `create` 함수를 호출해 결과값을 반환. 그렇지 않은 경우에는 기존 함수의 결과값 반환
    
    - `useMemo`로 전달된 함수는 렌더링이 일어나는 동안 실행됨
    
    - 렌더링이 일어나는 동안 실행돼서는 안될 작업(ex. side effect)을 `useMemo`의 함수로 넣으면 안 됨
  
  > - 의존성 배열 X : 렌더링 일어날 때마다 `create` 함수 실행됨 (의미X)
  >   
  >   ```jsx
  >   const memoizedValue = useMemo(
  >       () => computeExpensiveValue(a, b)
  >   );
  >   ```
  > 
  > - 의존성 배열에 빈 배열`[]` : 컴포넌트 마운트 시에만 `create` 함수 호출 됨 (마운트 이후에는 값 변경 X)
  >   
  >   ```jsx
  >   const memoizedValue = useMemo(
  >       () => {
  >             return computeExpensiveValue(a, b);
  >         },
  >         []
  >   );
  >   ```

- <mark>`useCallback()`</mark>
  
  : `useMemo()` Hook과 유사하지만 값이 아닌 함수를 반환
  
  - 컴포넌트가 렌더링될 때마다 매번 함수를 새로 정의하는 것이 아니라 **의존성 배열의 값이 바뀐 경우에만** 함수를 새로 정의해 리턴
  
  - 사용법
    
    ```jsx
    const memoizedCallback = useCallback(
        () => {
            doSomething(의존성 변수1, 의존성 변수2);
        },
        [의존성 변수1, 의존성 변수2]
    );
    ```
  
  - `useCallback(함수, 의존성 배열);` & `useMemo(() => 함수, 의존성 배열);` : 동일한 역할
  
  > ```jsx
  > import { useState } from "react";
  > 
  > function ParentComponent(props) {
  >     const [count, setCount] = useState(0);
  > 
  >     /** useCallback 사용X
  >     컴포넌트 내에서 정의한 함수를 자식 컴포넌트의 props로 넘겨 사용하는 경우
  >     부모 컴포넌트가 재렌더링될 때마다 매번 자식 컴포넌트도 다시 렌더링 됨
  >     const handleClick = (event) => {
  >         // 클릭 이벤트 처리
  >     };
  >     **/
  > 
  >     // useCallback 함수 사용해 컴포넌트가 마운트될 때만 함수가 정의됨
  >     const handleClick = useCallback((event) => {
  >         // 클릭 이벤트 처리
  >     }, []);
  > 
  >     return (
  >         <div>
  >             <button
  >                 onClick={() => {
  >                     setCount(count+1);
  >                 }}
  >             >
  >                 {count}
  >             </button>
  > 
  >             <ChildComponent handleClick={handleClick} />
  >         </div>
  >     );
  > }
  > ```

- <mark>`useRef()`</mark>
  
  : Reference를 사용하기 위한 Hook
  
  - Reference : 특정 컴포넌트에 접근할 수 있는 객체
  
  - `refObject.current` : 현재 레퍼런스하고 있는 element를 의미
  
  - 사용법 : `const refContainer = useRef(초기값);`
    
    - 파라미터로 초기값을 넣으면 해당 초기값으로 초기화된 레퍼런스 객체를 반환
    
    - 반환된 레퍼런스 객체는 컴포넌트의 라이프타임 전체에 걸쳐 유지됨(마운트 해제 전까지 유지) -> 매번 렌더링될 때마다 항상 같은 레퍼런스 객체 반환
    
    - 변경 가능한 `current` 라는 속성을 가진 하나의 상자
  
  > ```jsx
  > function TextInputWithFocusButton(props) {
  >     const inputElem = useRef(null);
  > 
  >     const onButtonClick = () => {
  >         // 'current'는 마운트된 input element를 가리킴
  >         inputElem.current.focus();
  >     };
  > 
  >     return (
  >         <>
  >             <input ref={inputElem} type="text" />
  >             <button onClick={onButtonClick}>
  >                 Focus the input
  >             </button>
  >         </>
  >     );
  > }
  > ```
  > 
  > - 버튼 클릭 시 input에 포커스를 하는 코드
  > 
  > - 초기값 `null`, 결과로 반환된 `inputElem` 레퍼런스 객체를 `input` 태그에 넣음
  > 
  > - 버튼 클릭 시 호출되는 함수에서 `inputElem.current`를 통해 실제 엘리먼트에 접근해 포커스 함수 호출
  > 
  > - `<div ref={myRef} />`
  >   
  >   - 노드가 변경될 때마다 `myRef`의 current 속성에 현재 해당되는 DOM 노드를 저장
  >   
  >   - `useRef` Hook은 위 ref 속성과 기능은 비슷하지만, 클래스의 instance 필드를 사용하는 것과 유사하게 다양한 변수 저장 가능
  
  - 내부의 데이터가 변경되었을 때 별도로 알리지 않음 -> current 속성을 변경한다고 재렌더링 일어나지 X
  
  - ref에 DOM 노드가 연결되거나 분리되었을 경우, 어떤 코드를 실행하고 싶다면 콜백 ref 사용해야함
  
  - callback Ref
    
    > ```jsx
    > function MeasureExample(props) {
    >     const [height, setHeight] = useState(0);
    > 
    >     const measureRef = useCallback(node => {
    >         if (node !== null) {
    >             setHeight(node.getBoundingClientRect().height);
    >         }
    >     }, []);
    > 
    >     return (
    >         <>
    >             <h1 ref={measureRef}>안녕, 리액트</h1>
    >               <h2> 위 헤더의 높이는 {Math.round(height)}px 입니다.</h2>
    >           </>
    >       );
    > }
    > ```
    > 
    > - `useRef` Hook을 사용하면, 레퍼런스 객체가 current 속성이 변경되었는지를 따로 알려주지 않음
    > - `useCallback` 함수 사용하면 자식 컴포넌트 변경되었을 때 알림받을 수 있고, 다른 변수에 반영 가능

- Hook의 규칙
  
  1. Hook은 무조건 **최상위 레벨에서만** 호출해야 함
  
  2. Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 함
  
  3. **리액트 함수 컴포넌트에서만** Hook을 호출해야 함
  - `eslint-plugin-react-hooks` plugin
    
    : Hook의 규칙을 강제로 따르게 하는 plugin

- Custom Hook 만들기
  
  : 이름이 `use`로 시작하고, 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수
  
  - 예제 - `isOnline` status에 따라 온라인 여부를 보여주는 `userStatus` 컴포넌트
    
    > ```jsx
    > import React, { useState, useEffect } from "react";
    > 
    > function UserStatus(props) {
    >     const [isOnline, setIsOnline] = useState(null);
    > 
    >     useEffect(() => {
    >         function handleStatusChange(status) {
    >             setIsOnline(status.isOnline);
    >         }
    > 
    >         ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange);
    >         return () => {
    >             ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
    >         };
    >     });
    > 
    >     return (
    >         <li style={{ color: isOnline ? 'green' : 'black' }}>
    >             {props.user.name}
    >         </li>
    >     );
    > }
    > ```
    > 
    > - 중복되는 로직을 `useUserStatus` 라는 Custom Hook으로 추출
    >   
    >   ```jsx
    >   import React, { useState, useEffect } from "react";
    >   
    >   function useUserStatus(userId) {
    >       const [isOnline, setIsOnline] = useState(null);
    >   
    >       useEffect(() => {
    >           function handleStatusChange(status) {
    >               setIsOnline(status.isOnline);
    >           }
    >   
    >           ServerAPI.subscribeUserStatus(userId, handleStatusChange);
    >           return () => {
    >               ServerAPI.unsubscribeUserStatus(userId, handleStatusChange);
    >           };
    >       });
    >   
    >       return isOnline;
    >   }
    >   ```
    > 
    > - Custom Hook 사용
    >   
    >   ```jsx
    >   function UserStatus(props) {
    >       const isOnline] = useUserStatus(props.user.id);
    >   
    >       if (isOnline === null) {
    >           return '대기중...';
    >       }
    >       return isOnline ? '온라인' : '오프라인';
    >   }
    >   
    >   function UserListItem(props) {
    >       const isOnline = useUserStatus(props.user.id);
    >   
    >       return (
    >           <li style={{ color: isOnline ? 'green' : 'black' }}>
    >               {props.user.name}
    >           </li>
    >       );
    >   }
    >   ```

---

## Event Handler (= Event Listner)

: Event(특정 사건)을 처리하는 것. 어떤 사건이 발생하면, 사건을 처리하는 역할

- camel case로 표기. `{}`로 감싸 함수 그대로 전달

- 예제 - Class Component
  
  > ```jsx
  > class Toggle extends React.Component {
  >     constructor(props) {
  >         super(props);
  >         this.state = { isToggleOn: true };
  >         // callback에서 'this'를 사용하기 위해서는 바이딩 필수
  >         this.handleClick = this.handleClick.bint(this);
  >     }
  > 
  >     handleClick() {
  >         this.setState(prevState => ({
  >             isToggleOn: !prevState.isToggleOn
  >         }));
  >     }
  > 
  >     render() {
  >         return (
  >             <button onClick={this.handleClick}>
  >                 {this.state.isToggleOn ? '켜짐' : '꺼짐'}
  >             </button>
  >         );
  >     }
  > }
  > ```
  > 
  > - 자바스크립트에서는 기본적으로 클래스 함수들이 바운드되지 않기 때문에 `bind` 해야 함
  >   
  >   - `bind`하지 않는다면 `this.handleClick`은 글로벌스코프에서 호출됨
  >   
  >   - 글로벌스코프에서 `this.handleClick`은 `undefined`이므로 사용불가
  > 
  > - `bind` 대신 Class fields syntax 사용 가능
  >   
  >   ```jsx
  >   class Mybutton extends React.Component {
  >       handleClick = () => {
  >           console.log('this is:', this); // Class fields syntax
  >       }
  >   
  >       render() {
  >           return (
  >               <button onClick={this.handleClick}>
  >                   클릭
  >               </button>
  >           );
  >       }
  >   }
  >   ```
  > 
  > - `bind`도, Class fields syntax도 사용X
  >   
  >   ```jsx
  >   class Mybutton extends React.Component {
  >       handleClick = () => {
  >           console.log('this is:', this);
  >       }
  >   
  >       render() {
  >           // 이렇게 하면 'this'가 바운드됨
  >           return (  // Arrow function 사
  >               <button onClick={() => this.handleClick()}>
  >                   클릭
  >               </button>
  >           );
  >       }
  >   }
  >   ```
  >   
  >   - `Mybutton` 컴포넌트가 렌더링도리 때마다 다른 콜백 함수가 생성되는 문제 발생
  >   
  >   - 이 콜백 함수가 하위 컴포넌트에 props로 넘겨지게 되면 하위 컴포넌트에서 추가 렌더링이 발생하게 됨

- 예제 - Function Component
  
  > ```jsx
  > class Toggle(props) {
  >     const [isToggleOn, setIsToggleOn] = useState(true);
  > 
  >     // 방법 1. 함수 안에 함수로 정의
  >     function handleClick() {
  >         setIsToggleOn((isToggleOn) => !isToggleOn);
  >     }
  > 
  >     // 방법 2. arrow function을 사용해 정의
  >     const handleClick = () => {
  >         setIsToggleOn((isToggleOn) => !isToggleOn);
  >     }
  > 
  >     return (
  >         <button onClick={handleClick}>
  >             {isToggleOn ? '켜짐' : '꺼짐'}
  >         </button>
  >     );
  > }
  > ```

- Event Handler에 Arguments 전달하기
  
  - Arguments(=Parameter. 매개변수) : 함수(Event Handler)에 주장할 내용(데이터)
  
  - 예제 - Class Component
    
    > ```jsx
    > <button onClick={(event) => this.deleteItem(id, event)}>삭제하기</button>
    > 
    > <button onClick={this.deleteItem.bind(this, id)}>삭제하기</button>
    > ```
  
  - 예제 - Function Component
    
    > ```jsx
    > functino MyButton(props) {
    >       const handleDelete = (id, event) => {
    >           console.log(id, event.target);
    >     };
    > 
    >     return (
    >         <button onClick={(event) => handleDelete(1, event)}>
    >               삭제하기
    >           </button>
    >     );
    > }
    > ```

---

## Conditional Rendering

: 조건에 따른 렌더링. 조건부 렌더링. 어떠한 조건에 따라서 렌더링이 달라지는 것

- 예제
  
  > ```jsx
  > function Greeting(props) {
  >     const isLoggedIn = props.isLoggedIn;
  > 
  >     if(isLoggedIn) {
  >         return <UserGreeting />;
  >     }
  >     return <GuestGreeting />;
  > }
  > ```

- JavaScript의 Truthy와 Falsy
  
  - Truthy : true는 아니지만 true로 여겨지는 값
  
  - Falsy : false는 아니지만 false로 여겨지는 값
  
  > ```js
  > // truthy
  > true
  > {} (empty object)
  > [] (empty array)
  > 42 (number, not zero)
  > "0", "false" (string, not empty)
  > 
  > //falsy
  > false
  > 0, -0 (zero, minus zero)
  > 0n (BigInt zero)
  > '', "", `` (empty string)
  > null
  > undefined
  > NaN (not a number)
  > ```

- Element Variables
  
  : React의 엘리먼트를 변수처럼 다룸
  
  > ```jsx
  > function LoginControl(props) {
  >     const [isLoggedIn, setIsLoggedIn] = useState(false);
  > 
  >     const handleLoginClick = () => {
  >         setIsLoggedIn(true);
  >     }
  >     const handleLogoutClick = () => {
  >         setIsLoggedIn(false);
  >     }
  > 
  >     // button은 컴포넌트로부터 생성된 리액트 엘리먼트
  >     let button;
  >     if (isLoggedIn) {
  >         button = <LogoutButton onClick={handleLogoutClick} />;
  >     } else {
  >         button = <LoginButton onClick={handleLoginClick} />;
  >     }
  > 
  >     return (
  >         <div>
  >             <Greeting isLoggedIn={isLoggedIn} />
  >                 {button}
  >         </div>
  >     )
  > }
  > ```

- Inline Conditions
  
  : 조건문을 코드 안에 집어넣는 것
  
  - Inline if : if문을 필요한 곳에 직접 넣어 사용. `&&` 연산자 사용
    
    - Short Circuit Evaluation
      
      - `true && expression` -> `expression`
      
      - `false && expression` -> `false`
    
    > ```jsx
    > function Mailbox(props) {
    >     const unreadMessages = props.unreadMessages;
    > 
    >     return (
    >         <div>
    >             <h1>안녕하세요!</h1>
    >               {unreadMessages.length > 0 &&
    >                   <h2>
    >                       현재 {unreadMessages.length}개의 읽지 않은 메세지가 있습니다.
    >                   </h2>
    >               }
    >           </div>
    >     );
    > }
    > ```
  
  - Inline If-Else : `?` 연산자(삼항 연산자 `condition ? true : false`) 사용
    
    > ```jsx
    > functio UserStatus(props) {
    >     return (
    >         <div>
    >             이 사용자는 현재 <b>{props.isLoggedIn ? '로그인' : '로그인하지 않은'}</b> 상태입니다.
    >         </div>
    >     )
    > }
    > ```

- Component 렌더링 막기
  
  : `null`을 리턴하면 렌더링하기 않음
  
  > ```jsx
  > function WarningBanner(props) {
  >     if (!props.warning) {
  >         return null;
  >     }
  >     return (
  >         <div>경고!</div>
  >     );
  > }
  > ```

---

## List

: 목록. 배열

- Key : 각 객체나 아이템을 구분할 수 있는 고유한 값

- 여러 개의 Component 렌더링하기 - `map()` 함수
  
  - 예제
    
    > - 곱하기 2
    >   
    >   ```jsx
    >   const doubled = numbers.map((number) => number * 2);
    >   ```
    > 
    > - 엘리먼트 렌더링
    >   
    >   ```jsx
    >   const numbers = [1, 2, 3, 4, 5];
    >   const listItems = numbers.map((number) =>
    >       <li>{number}</li>
    >   );
    >   
    >   ReactDOM.render(
    >       <ul>{listItems}</ul>
    >       document.getElementById('root')
    >   );
    >   ```
    > 
    > - 기본적인 List Component 
    >   
    >   - **elements 사이에서 고유한 `key` 필수!**
    >   
    >   - 두 List 사이에서는 Key가 같아도 상관없음
    >   
    >   ```jsx
    >   function NumberList(props) {
    >       const { numbers } = props;
    >   
    >       const listItems = numbers.map((number) =>
    >           <li ㅏ>{number}</li>
    >       );
    >       return (
    >           <ul>{listItems}</ul>
    >       );
    >   }
    >   
    >   const numbers = [1, 2, 3, 4, 5];
    >   ReactDOM.render(
    >       <NumberList numbers={numbers} />
    >       document.getElementById('root')
    >   );
    >   ```

---

## Form

: 사용자로부터 입력을 받기 위해 사용

- Controlled Components
  
  : 사용자가 입력한 값에 접근하고 제어할 수 있도록 해주는 컴포넌트
  
  : 값이 리액트의 통제를 받는 Input Form Element
  
  - 모든 데이터를 state에서 관리
  
  - 예제
    
    > ```jsx
    > function NameForm(props) {
    >     const [value, setValue] = useState('');
    > 
    >     const handleChange = (event) => {
    >         setValue(event.target.value);
    >     }
    > 
    >     const handleSubmit = (event) => {
    >         alert('입력한 이름: ' + value);
    >         event.preventDefault();
    >     }
    > 
    >     return (
    >         <form onSubmit={handleSubmit}>
    >             <label>
    >                 이름:
    >                   <input type="text" value={value} onChange={handleChange} />
    >               </label>
    >             <button type="submit">제출</button>
    >          </form>
    >     )
    > }
    > ```

- `Textarea` 태그
  
  : 여러 줄에 걸쳐 긴 텍스트를 입력받기 위한 HTML 태그
  
  > ```jsx
  > <textarea value={value} onChange={handleChange} />
  > ```

- `Select` 태그
  
  : Drop-down 목록을 보여주기 위한 HTLM 태그
  
  > ```jsx
  > <select value={value} onChange={handleChange}>
  >     <option value="apple">사과</option>
  >     <option value="banana">바나나</option>
  >     <option value="grape">포도</option>
  >     <option value="watermelon">수박</option>
  > </select>
  > ```
  > 
  > - 여러 개의 옵션 선택 가능
  >   
  >   ```jsx
  >   <select multiple={true} value{['B', 'C']}>
  >   ```

- `File input` 태그
  
  : 디바이스의 저장 장치로부터 하나 또는 여러 개의 파일을 선택할 수 있게 해주는 HTML 태그
  
  - `File input` 태그는 그 값이 읽기 전용이기 때문에 uncontrolled 컴포넌트(값이 React의 통제를 받지 않음)
  
  - 여러개의 입력 -> 여러 개의 state를 선언하여 각각의 입력에 대해 사용(`useState` 함수 여러개)

- Input Null Value
  
  : Value prop은 넣되 자유롭게 입력할 수 있게 만들고 싶다면 값에 `undefined` 또는 `null` 입력
  
  - 예제 - 처음에는 input 값이 'hi'로 정해져 있어 값을 바꿀 수 없는 입력 불가 상태였다가, 타이머에 의해 1초 뒤에 `value`가 `null`인 input 태그가 렌더링 되며 입력 가능해짐
  
  > ```jsx
  > ReactDOM.render(<input value="hi" />, rootNode);
  > 
  > setTimeout(function() {
  >     ReactDOM.render(<input value={null} />, rootNode);
  > }, 1000);
  > ```

---

## Lifting State Up

- Shared State
  
  : 하나의 데이터를 여러 개의 컴포넌트에서 표현해야 하는 경우, 가장 가까운 공통된 부모 컴포넌트의 state를 공유해 사용

- 예제
  
  > 1. 기본 코드
  >    
  >    ```jsx
  >    function BoilingVerdict(props) {
  >        if (props.celsius >= 100) {
  >            return <p>물이 끓습니다.</p>
  >        }
  >        return <p>물이 끊지 않습니다.</p>
  >    }
  >    ```
  >    
  >    ```jsx
  >    function Calculator(props) {
  >        const [temperature, setTemperature] = useState('');
  >    
  >        const handleChange = (event) => {
  >            setTemperature(event.target.value);
  >        }
  >    
  >        return (
  >            <fieldset>
  >                <legend>섭씨 온도를 입력하세요:</legend>
  >                <input
  >                    value={temperature}
  >                    onChange={handleChange} />
  >                <BoilingVerdict
  >                    celsius={parseFloat(temperature)} />
  >            </fieldset>
  >        )
  >    }
  >    ```
  > 
  > 2. 입력 컴포넌트 추출하기
  >    
  >    ```jsx
  >       const scaleNames = {
  >        c: '섭씨',
  >        f: '화씨'
  >    }
  >    
  >    function TemperatureInput(props) {
  >        const [temperature, setTemperature] = useState('');
  >    
  >        const handleChange = (event) => {
  >            setTemperature(event.target.value);
  >        }
  >    
  >        return (
  >            <fieldset>
  >                <legend>
  >                    온도를 입력해주세요(단위:{scaleNames[props.scale]});
  >                </legend>
  >                <input
  >                    value={temperature}
  >                    onChange={handleChange} />
  >            </fieldset>
  >        )
  >    }
  >    ```
  > 
  > 3. `Calculator` 함수 변경
  >    
  >    ```jsx
  >    function Calculator(props) {
  >        return (
  >            <div>
  >                <TemperatureInput scale="c" />
  >                <TemperatureInput scale="f" />
  >            </div>
  >        );
  >    }
  >    ```
  > 
  > 4. 사용자가 입력하는 온도 값이 temperature input의 state에 저장되기 때문에 섭씨 온도와 화씨 온도 값을 따로 입력받으면 2개의 값이 다를 수 있음 -> 값 동기화 시키기
  > 
  > 5. 온도 변환 함수 작성하기
  >    
  >    ```jsx
  >    function toCelsius(fahrenheit) {
  >        return (fahrenheit - 32) * 5 / 9;
  >    }
  >    
  >    function toFahrenheit(celsius) {
  >        return (celsius * 9 / 5) + 32;
  >    }
  >    ```
  > 
  > 6. `tryConvert` 함수 작성하기
  >    
  >    ```jsx
  >    function tryConvert(temperature, convert) {
  >        const input = parseFloat(temperature);
  >        if (Number.isNaN(input)) {
  >            return '';
  >        }
  >        const output = convert(input);
  >        const rounded = Math.rount(output * 1000) / 1000;
  >        return rounded.toString();
  >    }
  >    ```
  > 
  > 7. Shared State 적용하기
  >    
  >    ```jsx
  >    function TemperatureInput(props) {
  >        const handleChange = (event) => {
  >            props.onTemperatureChange(event.target.value);
  >        }
  >    
  >        return (
  >            <fieldset>
  >                <legend>
  >                    온도를 입력해세요(단위:{scaleNames[props.scale]});
  >                </legend>
  >                <input value={props.temperature} onChange={handleChange} />
  >            </fieldset>
  >        )
  >    }
  >    ```
  >    
  >    ```jsx
  >    function Calculator(props) {
  >        const [temperature, setTemperature] = useState('');
  >        const [scale, setScale] = useState('c');
  >    
  >        const handleCelsiusChange = (temperature) => {
  >            setTemperature(temperature);
  >            setScale('c');
  >        }
  >        const handleFahrenheitChange = (temperature) => {
  >            setTemperature(temperature);
  >            setScale('f');
  >        }
  >    
  >        const celsius = scale === 'f' ? tryConvert((temperature, toCelcius) : temperature;
  >        const fahrenheit = scale === 'c' ? tryConvert((temperature, toFahrenheit) : temperature;
  >    
  >        return (
  >            <div>
  >                <TemperatureInput scale="c" temperature={celsius} onTemperatureChange={handleCelsiusChange} />
  >                <TemperatureInput scale="f" temperature={fahrenheit} onTemperatureChage={handleFahrenheitChange} />
  >            </div>
  >        );
  >    }
  >    ```

---

## Composition vs Inheritance

- #### Composition
  
  : 여러 개의 새로운 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것 (합성)
  
  - Containment 
    
    : 하위 컴포넌트를 포함하는 형태의 합성 방법
    
    - Sidebar나 Dialog같은 box 형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없음
    
    - React component의 props에 기본적으로 들어있는 `children` prop 사용해 조합
    
    - 예제 - children prop을 사용한 FancyBorder 컴포넌트
      
      > ```jsx
      > functionj FancyBorder(props) {
      >     return (
      >         <div className={'FancyBorder FancyBorder-' + props.color}>
      >             {props.children}
      >         </div>
      >     );
      > }
      > ```
    
    - 여러 개의 children 집합이 필요한 경우 : 별도로 props를 정의해 각각 원하는 컴포넌트를 넣어주면 됨
      
      > ```jsx
      > function SplitPane(props) {
      >     return (
      >         <div className="SplitPane">
      >             <div className="SplitPane-left">
      >                 {props.left}
      >             </div>
      >             <div clasName="SplitPane-right">
      >                 {props.right}
      >             </div>
      >         </div<
      >     );
      > }
      > 
      > function App(props) {
      >     return (
      >         <SplitPane
      >             left={<Contacts />}
      >             right={Chat />}
      >         />
      >     );
      > }
      > ```
  
  - Specialization
    
    : 전문화, 특수화. 범용적인 개념을 구별이 되게 구체화하는 것
    
    - 기존의 객체지향 언어에서는 상속(Inheritance)을 사용하여 Specialization을 구현
    
    - 예제
      
      > ```jsx
      > function Dialog(props) {
      >     return (
      >         <FancyBorder color="blue">
      >             <h1 className="Dialog-title">
      >                 {props.title}
      >             </h1>
      >             <p className="Dialog-message">
      >                 {props.message}
      >             </p>
      >         </FancyBorder>
      >     );
      > }
      > 
      > function WelcomeDialog(props) {
      >     return (
      >         <Dialog
      >             title="어서오세요"
      >             message="우리 사이트에 방문하신 것을 환영합니다!"
      >         />
      >     );
      > }
      > ```
  
  - Containment와 Specialization 같이 사용하기
    
    > ```jsx
    > function Dialog(props) {
    >     return (
    >         <FancyBorder color="blue">
    >             <h1 className="Dialog-title">
    >                 {props.title}
    >             </h1>
    >             <p className="Dialog-message">
    >                 {props.message}
    >             </p>
    >             {props.children}
    >         </FancyBorder>
    >     );
    > }
    > ```
    > 
    > ```jsx
    > functino SignUpDialog(props) {
    >     const [nickname, setNickname] = useState('');
    > 
    >     const handleChange = (event) => {
    >         setNickname(event.target.value);
    >     }
    >     const handleSignUp = () => {
    >         alert(`어서오세요, ${nickname}님!`);
    >     }
    > 
    >     return (
    >         <Dialog
    >               title="화성 탐사 프로그램"
    >               message="닉네임을 입력해주세요.">
    >             <input
    >                 value={nickname}
    >                    onChange={handleChange} />
    >               <button onClick={handleSignUp}>
    >                 가입하기
    >             </button>
    >         </Dialog>
    >    );
    > }
    > ```

- #### Inheritance
  
  : 다른 컴포넌트로부터 상속을 받아서 새로운 컴포넌트를 만드는 것
  
  - Inheritance 보다 Composition 사용하기

- 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 만들고, 만든 컴포넌트들을 조합해서 새로운 컴포넌트를 만들자!

---

## Context

: React 컴포넌트들 사이에서 데이터를 기존의 props를 통해 전달하는 방식 대신 컴포넌트 트리를 통해 곧바로 컴포넌트로 전달

- 어떤 컴포넌트든지 데이터에 쉽게 접근 가능

- Context 언제 사용?
  
  - 여러 개의 Component들이 접근해야 하는 데이터
  
  - ex) 로그인 여부, 로그인 정보, UI 테마, 현재 언어 등
  
  > ```jsx
  > const ThemeContext = React.createContext('light');
  > 
  > // Provider을 사용해 하위 컴포넌트들에게 현재 테마 데이터 전달
  > // 모든 하위 컴포넌트들은 컴포넌트 트리 하단에 얼마나 깊이 있는지에 관계없이 데이터 읽을 수 있음
  > function App(props) {
  >     return (
  >         <ThemeContext.Provider value="dark">
  >             <Toolbar />
  >         </ThemeContext.Provider>
  >     );
  > }
  > 
  > // 중간에 위치한 컴포넌트는 테마 데이터를 하위 데이터에 전달할 필요 없음
  > function Toolbar(props) {
  >     return (
  >         <div>
  >             <ThemedButton />
  >         </div>
  >     );
  > }
  > 
  > function ThemedButton(props) {
  >     // 리액트는 가장 가까운 상위 테마 Provider를 찾아 해당되는 값 사용
  >     // 만약 해당되는 Provider가 없을 경우 기본값('light') 사용
  >     // 여기서는 상위 Provider가 있기 때문에 현재 테마의 값은 'dark'
  >     return (
  >         <ThemeContext.Consumer>
  >             {value => <Button theme={value} />}
  >         </Themecontext.Consumer>
  >     );
  > }
  > ```

- Context를 사용하기 전에 고려할 점
  
  - 컴포넌트와 Context가 연동되면 재사용성이 떨어짐
  
  - 다른 레벨의 많은 컴포넌트가 데이터를 필요로 하는 경우가 아니라면, 기존에 사용하던 방식대로 props를 통해 데이터를 전달하는 Component Composition 방법이 더 적합

- #### Context API
  
  - Context 생성
    
    - ```jsx
      const MyContext = React.createContext(기본값);
      ```
    
    - 기본값으로 `undefined`를 넣으면 기본값이 사용되지 않음
  
  - `Context.Provider`
    
    - `Context.Provider`컴포넌트로 하위 컴포넌트들을 감싸주면 모든 하위 컴포넌트들이 해당 컨텍스트의 데이터에 접근할 수 있음
    
    - ```jsx
      <MyContext.Provider value={/* some value*/}>
      ```
    
    - `value`라는 prop은 Provider 컴포넌트 하위에 있는 컴포넌트들에게 전달됨
      
      -> 하위 컴포넌트들이 소비한다는 의미에서 '컨슈밍 컴포넌트'라 함
    
    - 컨슈밍 컴포넌트 : context의 값의 변화를 지켜보다가 만약 값이 변경되면 재렌더링됨
    
    - 하나의 Provider 컴포넌트는 여러 개의 컨슈밍 컴포넌트와 연결될 수 있음
    
    - 여러개의 Provider 컴포넌트는 중첩되어 사용 가능
    
    - Provider value에서 주의해야 할 사항
      
      - Provider 컴포넌트가 재렌더링될 때마다 모든 하위 consumer 컴포넌트가 재렌더링됨
        
        - state를 사용해 불필요한 재렌더링 막음
          
          > - 불필요하게 재렌더링 발생
          >   
          >   ```js
          >   function App(props) {
          >       return (
          >           <MyContext.Provider value={{ something: 'something' }}>
          >               <Toolbar />
          >           </MyContext.Provider>
          >       );
          >   }
          >   ```
          > 
          > - 해결
          >   
          >   ```jsx
          >   function App(props) {
          >       const [value, setValue] = useState({ something: 'something' });
          >   
          >       return (
          >           <MyContext.Provider value={value}>
          >               <Toolbar />
          >           </MyContext.Provider>
          >       );
          >   }
          >   ```
  
  - `Class.contextType` : Provider 하위에 있는 class 컴포넌트에서 context의 데이터에 접근하기 위해 사용 (현재 사용 X)
  
  - `Context.Consumer`
    
    : context의 데이터를 구독하는 컴포넌트
    
    - Class 컴포넌트에서는 `Class.contextType` 사용
    
    - Function 컴포넌트에서는 `Context.Consumer` 사용해 context 구독
      
      > ```jsx
      > <MyContext.Consumer>
      >     // function as a child
      >     {value => /* 컨텍스트의 값에 따라서 컴포넌트들을 렌더링 */}
      > </MyContext.Consumer>
      > ```
    
    - function as a child(component의 자식)를 사용하는 방법
      
      > ```jsx
      > // children이라는 prop을 직접 선언하는 방식
      > <Profile children={name => <p>이름: {name}</p>} />
      > 
      > // Profile 컴포넌트로 감싸서 children으로 만드는 방식
      > <Profile>{name => <p>이름: {name}</p>}</Profile>
      > ```
  
  - `Context.displayName`
    
    : 문자열
    
    > ```jsx
    > const MyContext = React.createContext(/* some value */);
    > MyContext.displayName = 'MyDisplayName';
    > 
    > // 개발자 도구에 "MyDisplayName.Provider"로 표시됨
    > <MyContext.Provider>
    > 
    > // 개발자 도구에 "MyDisplayName.Consumer"로 표시됨
    > <MyContext.Consumer>
    > ```
  
  - 여러 개의 Context 사용하기 - `Context.Provider` 중첩 사용

- <mark>`useContext()`</mark>
  
  - 사용법
    
    ```jsx
    function MyComponent(props) {
        const value = useContext(MyContext);
    
        return (
            ...
        )
    }
    ```
  
  - ```jsx
    // 올바른 사용법
    usecontext(MyContext);
    
    // 잘못된 사용법
    useContext(MyContext.Consumer);
    useContext(Mycontext.Provider);
    ```

---

## CSS

- Selector
  
  - 클래스 : `.클래스명`
  
  - ID : `#ID명`
  
  - 전체 : `*`

- Element의 상태와 관련된 selector
  
  - `:hover` : 마우스 커서가 element 위에 올라왔을 때
  
  - `:active` : 주로 `<a>` 태그(link)에 사용되는데, element가 클릭됐을 때
  
  - `:focus` : 주로 `<input>` 태그에서 사용되는데, element가 초점을 갖고 있을 경우
  
  - `:checked` : radio button이나 checkbox같은 유형의 `<input>` 태그가 체크되어 있는 경우
  
  - `:first-child`, `:last-child` : 상위 element를 기준으로 각각 첫번째 child, 마지막 child일 경우
  
  - [참고문서1](https://www.w3schools.com/css/css_selectors/asp)
  
  - [참고문서2](https://www.w3schools.com/csSref/css_selectors.asp)

- 레이아웃과 관련된 속성
  
  - `display`
    
    - `display: none;` : element를 화면에서 숨기기 위해 사용
      
      - `<script>` 태그의 display 속성 기본값은 `display: none;`
    
    - `display: block;` : 블록 단위로 element를 배치 (전체 width 차지)
      
      - `<p>`, `<div>`. `<h1>`~`<h6>` 태그의 display 속성 기본값이 `display: block;`
    
    - `display: inline;` : element를 라인 안에 넣는 것
      
      - `<span>` 태그의 display 속성 기본값이 `display: inline;`
    
    - `display: flex;` : element를 블록 레벨의 flex container로 표시
      
      - container이기 때문에 내부에 다른 element들을 포함
  
  - `visibility`
    
    - `visibility: visible;` : element를 화면에 보이게 하는 것
    
    - `visibility: hidden;` : 화면에서 안 보이게 감추는 것. element를 안 보이게만 하는 것이고, 화면에서의 영역은 그대로 차지
  
  - `position`
    
    - `static` : 기본값으로 element를 원래의 순서대로 위치시킴
    
    - `fixed` : element를 브라우저 window에 상대적으로 위치시킴
    
    - `relative` : element를 보통의 위치에 상대적으로 위치시킴
    
    - `absolute` : element를 절대 위치에 위치시킴
  
  - 가로, 세로 길이와 관련된 속성
    
    - `width`, `height`, `min-width`, `min-height`, `max-height`, `max-height`
  
  - Flexbox
    
    - flex container 안 flex item들
    
    - `display: flex;` : element를 flex container로 사용하기 위해 선언
    
    - `flex-direction`
      
      - `row` : 기본값. 아이템을 행(row)을 따라 가로 순서대로 왼쪽부터 배치
      
      - `column` : 아이템을 열(column)을 따라 세로 순서대로 위쪽부터 배치
      
      - `row-reverse` : 아이템을 행(row)의 역(reverse) 방향으로 오른쪽부터 배치
      
      - `column-reverse` : 아이템을 열(column)의 역(reverse)방향으로 아래쪽부터 배치
      
      - flex-direction이 row인 경우 : main axis는 좌->우, cross axis는 위->아래
      
      - flex-direction이 column인 경우 : main axis는 위->아래, cross axis는 좌->우
    
    - `align-items` : cross axis 기준으로 정렬
      
      - `stretch` : 기본값으로써 아이템을 늘려서(stretch) 컨테이너를 가득 채움
      
      - `flex-start` : cross axis의 시작 지점으로 아이템을 정렬
      
      - `center` : cross axis의 중앙으로 아이템을 정렬
      
      - `flex-end` : cross axis의 끝 지점으로 아이템을 정렬
      
      - `baseline` : 아이템을 baseline 기준으로 정렬
    
    - `justify-content`
      
      - `flex-start` : main axis의 시작 지점으로 아이템을 맞춤
      
      - `center` : main axis의 중앙으로 아이템을 맞춤
      
      - `flex-end` : main axis의 끝 지점으로 아이템을 맞춤
      
      - `space-between` : main axis를 기준으로 첫 아이템은 시작 지점에 맞추고 마지막은 끝 지점에 맞추며, 중간에 있는 아이템들 사이(between)의 간격(space)이 일정하게 되도록 맞춤
      
      - `space-around` : main axis를 기준으로 각 아이템의 주변(around) 간격(space)을 동일하게 맞춤

- font와 관련된 대표적인 속성
  
  - `font-family` : 어떤 글꼴 사용할기
    
    - 띄어쓰기 들어갈 경우 `""`로 묶어주기
    
    - 여러개 쓰면 대비책으로 사용
    
    - 일반적인 글꼴 분류 (마지막에 작성)
      
      - `serif` : 각 글자의 모서리에 작은 테두리를 갖고 있는 형태의 글꼴
      
      - `sans-serif` : 모서리에 테두리가 없이 깔끔한 선을 가진 글꼴. 컴퓨터 모니터에서는 Serif보다 가독성이 좋음
      
      - `monospace` : 모든 글자가 같은 가로 길이를 가지는 글꼴. 주로 코딩할 때 사용
      
      - `cursive` : 사람이 쓴 손글씨 모양의 글꼴
      
      - `fantasy` : 장식이 들어간 형태의 글꼴
  
  - `font-size` : px(pixel), em, rem, vw(viewpoint width)
  
  - `font-weight` : bold, 숫자
  
  - `font-style`
    
    - `normal` : 일반적인 글자의 형태
    
    - `italic` : 기울어진 형태. 별도로 기울어진 형태의 글자들을 직접 디자인해 만든 것
    
    - `oblique` : 글자가 비스듬한 형태. 그냥 글자를 기울인 것

- styled-components
  
  - 설치
    
    ```jsx
    # npm 사용 시
    npm install --save styled-components
    
    # yarn 사용 시
    yarn add styled-components
    ```
  
  - 기본 사용법 - tagged template literal
    
    - Untagged template literal : `로 둘러쌓인 문자열
    
    - Tagged template literal
      
      - ```jsx
        myFunction`string text ${expression} string text`;
        ```
  
  - 예제  
    
    > ```jsx
    > import React from "react";
    > import styled from 'styled-components';
    > 
    > const Wrapper = styled.div`
    >     paddig: 1em;
    >     background: grey;
    > `;
    > 
    > const Title = styled.h1`
    >     font-size: 1.5em;
    >     color: whilte;
    >     text-align: center;
    > `;
    > 
    > funtion MainPage(props){
    >     return (
    >         <Wrapper>
    >             <Title>
    >                 안녕, 리액트!
    >               </Title>
    >           </Wrappe>
    >     );
    > }
    > 
    > export default Mainpage;
    > ```
  
  - styled-components 확장하기
    
    > ```jsx
    > import React from "react";
    > import styled from 'styled-components';
    > 
    > // Button 컴포넌트
    > const Button = styled.button`
    >     color: grey;
    >     border: 2px solid palevioletred;
    > `;
    > 
    > // Button에 style이 추가된 RoundedButton 컴포넌트
    > const RoundedButton = styled(Button)`
    >     border-radius: 16px;
    > `;
    > 
    > funtion Sample(props){
    >     return (
    >         <div>
    >             <Button>Normal</Button>
    >             <RoundedButton>Rounded</RoundedButton>
    >           </div>
    >     );
    > }
    > 
    > export default Sample;
    > ```

---

## 프로젝트 생성

1. `npx create-react-app 앱이름`

2. `cd 앱이름`

3. `npm start`

4. `npm install --save react-router-dom styled-components`
   
   - `--save` : 의존성 패키지에 저장

5. 컴포넌트 구상

6. `src/component` 폴더에 `list`, `page`, `ui`(사용자가 입력할 수 있는 컴포넌트) 등 하위 폴더 생성

7. 컴포넌트 구현

8. App.js에서 경로 구현
   
   - `react-router-dom` : 라우팅 구현 도와줌
     
     - `path` : 경로, `element` : 경로값이 일치할 경우 보여줄 element
   
   > ```jsx
   > <BrowserRouter>
   >     <Routes>
   >         <Route index element={<MainPage />} />
   >         <Route path="places" element={<PlacePage />} />
   >         <Route path="games" element={<GamePgae />} />
   >     </Routes>
   > </BrowserRouter>
   > ```
   
   - `useNavigae()` Hook
     
     > ```jsx
     > function SampleNavigate(props) {
     >     const navigate = useNavigate();
     > 
     >     const moveToMain = () => {
     >         navigate("/");
     >     }
     >     
     >     return (
     >         ...
     >     );
     > }
     > ```

9. index.js 파일 작성

10. `npm run build`로 Production 빌드
    
    : 코드와 애플리케이션이 사용하는 이미지, CSS 파일 등의 파일을 모두 모아서 패키징하는 과정

11. `npm install -g serve` 로 serve 설치

12. `serve -s build` 빌드 폴더 기반으로 웹 애플리케이션 serve

13. 배포
