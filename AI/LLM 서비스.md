# LLM 서비스

: Frontend + Backend + LLM API

- 우리가 개발할 부분은 Frontend + Backend

---

## Agent, RAG, ChatUI

- Agent
  
  : 요청을 확인하고 스스로 검색해서 답변 계획을 세우는(planning) 존재
  
  - OpenAI Swarm
    
    - 역할 부여하는 메시지 만들기
    
    - Agent가 할 수 있는 함수
      
      - `look_up_item`
      
      - `execute_refund`

- RAG (Retrieval Augmented Generation)
  
  - 유저로부터 Query를 받아 검색엔진(Retrieval model)에서 relevant docs(결과)를 LLM에 전달해 답변을 생성하는 것
  
  - OpenAI 모델을 사용하거나 직접 구현 (Embedding model)

- Vector DB
  
  - 질문과 docs를 벡터로 바꿔 두 벡터 간 유사도 비교를 통해 결과물을 가져옴
  
  - Pinecone, Chroma, PostgreSQL, pgvector 등

- 직접 구현하려면 필요한 것
  
  1. LLM API와 통신 코드 (OpenAI / Gemini / ...)
  
  2. PDF, HTML, ... 기타 자료를 LM에 넣기 위한 Document Parser (보토 700~2000 크기 벡터)
  
  3. Document Embedding을 위한 Chunking
  
  4. Chunk를 넣을 Embedding 모델 (로컬모델 or API)
  
  5. Embedding 저장한 Vector DB (Pinecone, PGVector, ...)
  
  6. 서버 준비 (Web + LLM + Vector)

- 최소한으로만 구현하려면 필요한 것
  
  1. LLM API와 통신 코드 (OpenAI / Gemini / ...)
  
  2. 서버 준비 (Web)
  - 이외에는 **OpenAI Assistant API**가 해줌
  
  - Rag를 시스템에 붙이고 Pinecone에 직접 사용하는 건 직접 해야 함

- Assistant API
  
  = OpenAI + Agent / RAG
  
  - embedding, chunking 등 뿐만 아니라 툴도 제공해줌
  
  - [Assistant 생성 문서](https://platform.openai.com/playground/assistants)
    
    - FIle search에 파일 업로드하면 RAG 가능 -> 자동으로 Vector store 생성 (하나의 파일에 대한 text 형식의 chunking, embedding 된 vectorDB를 만든 것)
  
  - [Chat UI by Hugging Face](https://hf.co/chat)
    
    - Fetch URL, Web Search 등 지원
    
    - 내부적으로 Function Calling을 사용해서 주어진 URL을 브라우징 후 가져와 응답 생성

---

## Frontend (Node.js)

- `node` 와 `npm` 명령어 사용

- 개발 패키지 설치
  
  > ```python
  > npm init -y
  > npm install -D parcel tailwindcss postcss autoprefixer
  > ```

---

## Backend (Python)

- `python` 과 `pip` 명령어 사용

- 개발 패키치 설치
  
  > ```python
  > pip install openai fastapi -U
  > ```

---

## 가장 간단한 Frontend 서버 띄워보기

1. [Repo](https://github.com/Beomi/ssafy-2024-frontend/tree/v1-basic-echo) 참고 폴더구조 생성

2. Tailwind config
   
   - `npx tailwindcss init` : `tailwind.config.js` 생성
   
   - `tailwind.config.js` 수정 - 어디에 Tailwind 적용할지
     
     > ```py
     > content: ["./index.html", "./src/**/*.{js, tx, jsx, tsx}"],
     > ```
   
   - `.postcssrc.json` 수정하기
     
     > ```py
     > {
     >     "plugins": {
     >         "tailwindcss": {}
     >     }
     > }
     > ```
   
   - `app/style.css` 수정하기

3. `npm command` 작성하기
   
   > `package.json`에서 아래 코드를 `scripts`에 추가하면 `npm start`로 서버 띄우기 -> `npm run build`로 빌드
   > 
   > ```py
   > "start": "parcel serve index.html",
   > "build": "parcel build index.html""
   > ```
