# React 환경 설정

1. Node.js 설치
   
   - LTS 버전 추천
   
   - 명령 프롬프트에 `node --version`, `npm --version` 치면 설치 확인 가능

2. VS Code 설치

3. Create React App (CRA)로 react 환경 구축
   
   > 1. CRA를 이용해 프로젝트 생성
   >    
   >    `$ npx create-react-app 프로젝트이름`
   >    
   >    `$ cd 프로젝트 이름`
   > 
   > 2. 생성한 프로젝트 실행
   >    
   >    `$ npm run start`
   > 
   > 3. 실행 종료
   >    
   >    `ctrl + c`

4. CRA 구조
   
   > - `npm run start` : 서버 실행
   > 
   > - `npm run build` : src 폴더 속 코드를 build 폴더로 합쳐줌
   > 
   > - `npm run test` : test 코드들 실행함
   > 
   > - `npm run eject` : react scripts 속 코드 밖으로 나옴. config 폴더속 `webpack.config.js` 파일 수정하고 싶을 때 씀
   > 
   > - `git reset --hard` : 되돌리기

5. eslint
   
   : 문법 및 코드 스타일을 검사해주는 도구
   
   - CRA에는 자체 내장되어 있음 (package.json의 eslintConfig)
   
   - eslint 커스텀하기
     
     > 1. `.eslintrc.json` 파일 생성
     > 
     > 2. `package.json` 속 `eslintConfig` 코드를 `.eslintrc.json` 파일로 옮기기
     > 
     > 3. 별도의 옵션 추가 시 `"rules"`로 따로 정의
     >    
     >    ```jsx
     >    # .eslintrc.json
     >    {
     >        "extends": [
     >            "react-app",
     >            "react-app/test"
     >        ],
     >        "rules": {
     >            "no-conlose": "error"  // console.log 사용 불
     >        }
     >    }
     >    ```

6. prettier
   
   : 자동으로 코드를 정해진 규칙에 맞게 고쳐주는 툴
   
   - VS Code - Extensions - prettier 검색 - install
   
   - VS Code 설정 - formatter 검색 - None을 prettier로 변경 - Editor:Format On Save 체크
   
   - `ctrl + s` 로 정렬 가능
   
   - prettier 커스텀하기
     
     > 1. `.prettierrc.json` 파일 생성
     > 
     > 2. 원하는 규칙 정의
     >    
     >    ```jsx
     >    # .prettierrc.json
     >    {
     >        "singleQuote": true,
     >        "semi": true,  // 세미콜론 찍어주
     >        "tabWidth": 2,
     >        "trailingComma": "all", // 마지막까지 comma 찍어주기
     >    }
     >    ```
