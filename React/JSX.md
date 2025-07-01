# JSX

= HTML + Javascript

- 특징
  
  - JSX에서 사용되는 태그의 속성 이름이 HTML과 조금 다름
    
    - `class` -> `className`
    
    - `for` -> `htmlFor`
    
    - `onclick` -> `onClick`
  
  - 태그를 명시적으로 닫아줘야 함
    
    - ex. `<input type="text" />`
  
  - 하나의 태그로 감싸져 있어야 함
    
    - 동시 나열하고 싶으면, 나열한 것들을 하나의 태그로 감싸줘야 함

- `{}` 안에 변수 넣으면 됨
  
  - `+` 등 연산도 가능
  
  - `true`, `false`, `undefined`, `null`은 아무런 내용도 출력되지 않음
  
  - object는 `{object}` 형식으로 사용할 수 없음. 단, `{arr}`(배열)은 가능
  
  - 속성으로 변수 넣을 수 있음
    
    - ex. `<img src={imageUrl} alt="logo" />`

- ### 조건문
  
  - 삼항 연산자
    
    - `{ 조건 ? 참일 때 : 거짓일 때}`
  
  - AND 연산자 : `&&`
    
    - 앞이 참이면 뒤 값이 됨. 앞이 거짓이면 `false`(빈 칸)
  
  - OR 연산자 : `||`
    
    - 앞이 참이면 뒤 값 검사X. 앞이 거짓이면 뒤 값이 됨
  
  - IF문 (즉시 실행 함수) - 잘 안 씀
    
    - ```jsx
      {(() => {
          if (1+1 === 2) return 'IF';
          else return 'ELSE';
      })()}
      ```

- ### 반복문
  
  - 각 요소에 키를 할당해주어야 함 (unique key)
    
    - React 내부적으로 구분하기 위해 필수적으로 넣어야 함
  
  - ```jsx
    <ul>
      <li>
        {arr.map((item, index) => {
          return <h4 key={`${item}-${index}`}>{item}</h4>;
        })}
      </li>
    </ul>
    ```
  
  - 인덱스를 키로 주면 요소들의 순서가 바꼈을 경우, 문제가 생길 수 있음 -> 인덱스를 키로 사용하는 것 권장 X

- ### JSX에 style 적용
  
  - 중괄호 `{}`로 적용
  
  - ```jsx
    const roundBoxStyle = {
      position: 'absoulute',
      top: 50,
      left: 50,
      width: '50%',
      height: '200px',
      padding: 20,
      background: 'rgba(162,216,235,0.6)',
      // 3. 속성은 camelCase
      borderRadius: 50
    };
    
    const element = (
        <div
          style={
            {
              // 1. Object로 CSS 작성
              position: 'relative',
              width: 400,
              height: 1000,
              background: '#f1f1f1'
            }
          }
        >
          <div style={roundBoxStyle}>Hello</div>
    
          <div style={{ ...roundBoxStyle, top: 350}}>
            { /* className을 통한 스타일링 (CSS-in-JS) */ }
            <div className={"highlight"}>Hello2</div>
            // index.css에서 'highlight' class에 대해 정의해놓으면 됨
          </div>
    
          <div style={{ ...roundBoxStyle, top: 650 }}>
            { /* 5. 조건적 스타일 */ }
            <div
              className={
                // 1 + 1 === 2? 'highlight' : false
                1 + 1 === 2 && 'highlight'
                // 1 + 1 === 2 ? 'highlight' : ''  바로 위 코드와 동일 작동
              }
            >
            Hello3
            </div>
          </div>
        </div>
    )
    ```

- ### 예시 - 구구단 출력
  
  > ```jsx
  > import ReactDom from 'react-dom';
  > 
  > const num = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  > 
  > const element = (
  >     <div style={{ display: 'flex' }}>
  >       {num.map(
  >         (n) => 
  >           n !== 1 &&
  >           n !== 5 && (
  >             <div style={{padding: 10, color: n % 2 ? "blue" : "black" }}>
  >               {num.map((m) => (
  >                 <div>
  >                   {n} x {m} = {n * m}
  >                 </div>
  >               ))}
  >             </div>
  >           )
  >       )}
  >     </div>
  > );
  > 
  > ReactDom.render(element, document.getElementById("root"));
  > ```
