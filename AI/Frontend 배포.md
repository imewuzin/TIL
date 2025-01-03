# Frontend 배포

## 사전준비

1. Github 가입 & Repository(public) 준비

## `Parcel.js`로 빌드하기

- `Parcel.js` = HTML / CSS / JS Builder

- 프론트엔드 빌드 = 여러개의 HTML / CSS / JS를 하나(혹은 N개)로 줄여서 압축

- `npm run build` 명령어로 빌드 가능

---

## 배포

- ### 방법 1 - 빌드한 웹앱 Github Pages에 배포
  
  1. `gh-pages` 패키지 설치
     
     > ```py
     > npm i -D gh-pages
     > ```
  
  2. `package.json`에 `dist` 폴더를 배포한다는 코드 추가
     
     > ```py
     > "homepage": "https://username.github.io/repository/",  # username, repository 수
     > "deploy": "gh-pages -d dist"
     > ```
     > 
     > 이후 `npm run deploy` 명령어로 Github pages에 배포
     > 
     > -> `gh-pages` 브랜치에 HTML / CSS / JS 업로드됨

- ### 방법 2 - Vercel에 배포
  
  - Frontend 전문. HTML / CSS / JS를 넘어 프론트 서버사이드 렌더링 (SSR)등이 가능
  
  - 간단한 API Wrapping은 Vercel만으로 가능
  1. Vercel 회원가입 (Github으로 가입)
  
  2. 작성한 코드 github 레포지토리에 올리기
     
     - default branch 유의하기!
     
     > gitignore에 아래코드 작성
     > 
     > ```py
     > .DS_Store
     > .parcel-cache/
     > dist/
     > node_modules/
     > ```
  
  3. Import Repository에서 레포 선택하고 Deploy 버튼 클릭하면 배포 완료
  
  4. 수정 원하면 git에 수정하고 `push` -> 반영됨

---

## Chat UI 웹앱 Vercel에 배포하기

- Vercel Chat UI
  
  : Vercel에서 개발한 AI Chatbot을 위한 템플릿
  
  - Next.js 기반으로 개발됨
  
  - [데모](https://chat.vercel.ai/) (gpt 4o-mini로 동작)
  
  - [레포](https://github.com/vercel/ai-chatbot)
1. [가장 간단한 Chat UI 배포](https://github.com/vercel/ai-chatbot?tab=readme-ov-file#deploy-your-own) 링크의 버튼 클릭

2. 새 레포 생성

3. Add Storage에 Neon, Blob Store 둘 다 Add
   
   - Blob Store은 계정당 1개 제공. 쓰고있는거에서 delete store로 삭제해야 사용 가능

4. Create Database에서 가장 가까운 서버인 Singapore 선택

5. Blob Store 이름 작성

6. Add Environment Variables 작성
   
   - [AUTH_SECRET](https://generate-secret.vercel.app/32) 들어가 나온 문자 복사
   
   - [OpenAI API Key](https://platform.openai.com/account/api-keys) 에서 생성
