# Backend 배포

- [예제 백엔드 레포](https://github.com/Beomi/ssafy-2024-backend)

## FastAPI

- Chat Completion API + Assistant API

| Chat Completion API              | Assistant API                       |
| -------------------------------- | ----------------------------------- |
| 주어진 Input에 대한 Output             | 주어진 Input에 대한 Output                |
| 채팅 내용에 대해 저장 X                   | 채팅 내용 저장 O (threads)                |
| RAG, 기타 함수 직접 구현 (직접 Prompt에 넣기) | RAG, Code Interpreter 등 특수 기능 사용 가능 |
| Function Calling으로 구현            | 사전에 지정한 Assistant                   |

---

## Fly.io

1. github로 가입

2. Organization을 Personal로 Lauch an app. 쭉 이어서 가기

3. [Flyctl 설치](https://fly.io/docs/flyctl/install/) (OS별로 다름!)

## Fly.io에 FastAPI 배포하기

1. `fly` 명령어 실행

2. Backend 폴더에서 `fly launch` 명령어 실행

3. 뜨는 웹에서 App name, region, Internal port 입력하면 배포 준비 끝

4. `fly deploy`로 배포 (이대로 하면 오류남. API Key 문제 등)

5. Fly.io에 환경 변수 설정하기
   
   - secrets탭에서 `OPENAI_API_KEY` 저장

6. `fly deploy`로 재배포 필수

---

## FastAPI API + Vercel FE 연동

1. JS 코드 폴더에 `.env.development`와 `.env.production` 생성
   
   - `.env.development`에 `API_ENDPOINT=http://localhost:8000`
   
   - `.env.production`에 `API_ENDPOINT=http://개인별URL나온것.fly.dev)
     
     - 개개인의 Fly.io에서 나온 URL로 설정

2. Frontend 재배포
   
   - GH-pages: `npm run build` & `npm run deploy`
   
   - Vercel: 단순 `git push`

3. 
