# git

- Distributed Version Control System (분산 버전 관리 시스템)

- 컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간의 작업을 조율

- 로컬 저장소를 사용하는 명령어 기반의 시스템

- Linus Torvalds, 2005 에 만듦

- 자격 증명 관리자 - github 자격 지워야 내 흔적 다 지우는 것!

---

## git과 github 차이점

- git : 로컬(내 컴퓨터)에서 사용하고 있는 프로그램

- github : git 프로젝트를 지원하는 웹 호스팅 서비스. 보안 조금 약함

- gitlab : 그다음 큰 클라우드. SSAFY에서 사용

- bitbucket : 그다음 큰 클라우드. 좀 구림(?)

- VCS (분산 버전 관리 시스템)의 장점
  
  - 각 파일을 이전의 상태로 되돌릴 수 있음
  - 프로젝트를 통째로 이전 상태로 되돌릴 수 있음
  - 시간에 따라 수정 내용을 비교해 볼 수 있음
  - 누가 문제를 일으켰는지 추적 가능
  - 누가 언제 만든 파일인지 이슈인지 알 수 있음
  - 파일을 잃어버리거나 잘못 고쳤을 때도 쉽게 복구 가능

- CVCS (중앙 집중식 버전관리)
  
  - 한 개의 서버에서만 버전관리
  - 서버가 망가지면 복가가 어려움

- DVCS (분산 버전 관리 시스템)
  
  - git과 github에 적용된 시스템
  - 버전관리를 한 개의 서버에서만 하지 않고 로컬 저장소에서도 함께 관리
  - 문제가 생기면 복제된 클라이언트 서버로 복원 가능

---

## local, .hit, repository

![git명령어](https://github.com/user-attachments/assets/0189c776-6c97-4fce-9ff1-6e4765c91159)

- **Working Area** : 빨간 글씨. Git의 .git 하면 생기는 공간. 
- **Staging Directory** : 초록 글씨. Git에서 Working Directory를 제외한 공간. 대기실이라 생각하면 되며, 생성/수정/삭제 이루어짐
- **Repository** : GitHub의 공간. local의 폴더와 연결됨. commit들이 쌓여있음.
* Repository와 local의 폴더명 같을 필요는 없지만, 같은게 편함
* commit : 변경된 파일들을 저장하는 행위, GIT에서 버전을 말하며 snapshot이라고도 함

---

## git 설정

- ***`git init`***
  
  : git을 시작할 때 가장 먼저 사용하는 명령어.
  
  - git으로 한 폴더를 버전관리 하겠다는 선언. 2개 동시ㄴㄴ
  
  - 하위 폴더까지 관리함.
  
  - git init 후에 가장 기본 브랜치인 *(master)* 가 터미널에 표시됨.
  
  - 아무데서나 사용하면 안되고 git으로 관리할 폴더를 생성하고 사용해야 함.
  
  - 숨김 파일로 생성되서 보려면 폴더→보기→숨김 파일 표시
  
  - <mark>git한 폴더 안에 또 git한 폴더 넣기 ㄴㄴ</mark>. 이후 행동이 아무 의미 없어짐!!
    
    > 해결책
    > 
    > : 폴더→보기→숨김 파일 누르면 /git 폴더 볼 수 있음. 이 폴더 지우면 git init한 것 취소됨. 수정은 절대 하면 안됨!

- ***`git remote`***
  
  : Repository 생성하고 로컬 저장소와 연결할 때마다 작업
  
  - *`git remote add origin <github url>`* (<>는 안 써도 됨)
  - origin은 깃허브의 원격 저장소 의미
  - 현재 git으로 관리를 선언한 로컬 저장소와 클라우드 호스팅으로 관리할 github의 연결
  - *`git remote -v`* : 연결되었는지 확인 가능
  - *`git remote remove origin`* : 연결 지우기
  - git에서 복사는 shift + insert

- ***`git config`***
  
  : 처음으로 git을 시작하거나 ,컴퓨터 세팅 시에 한번만 설정
  
  - *`git config --global [user.name](http://user.name) “<username>”`*
  - *`git config --global [user.email](http://user.email) “<email>”`*
  - *`git config --global -l`*  : git global 설정 보기
  - `--global` : 전역 설정, 컴퓨터 전체에
  - `--local` : 지역 설정, 폴더에서
  - *`git config --global [user.name](http://user.name), git config --global [user.email](http://user.email)`* 입력하면 입력한 username과 useremail 나옴

---

## git 명령어

- `git init` → `git remote add origin <url>` → `git add .` → `git status` : 파일 초록색으로 표시됨 → `git commit -m “hello “` → `git log --oneline` → `git push origin master`
  
  : 로컬과 git 연결. git의 Repositories 들어가면 로컬의 파일들 들어가있음.

- ***`git status`***
  
  : git이 관리하는 현황을 한 눈에 파악할 수 있음
  
  - 상황에 따라 출력 결과가 다름
  - working directory 단계에서 많이 사용됨
  - <mark>수시로 입력해 확인할 것!</mark>

- ***`git log`***
  
  : commit history 보여줌. git status는 현 상황을 보여준다면, git log는 저자, 날짜, commit의 message를 보여줌.

- ***`git add`***
  
  : staging area로 올리는 명령어. 단독 사용X
  
  - *`add .`* : 전체 파일 add
  - *`add 파일이름.확장자`* : 한 파일 add
  - 여러개의 파일을 add하고 싶으로 공백문자(스페이스바)로 파일 이름 구분
  - 파일 이름에 공백문자가 있다면 따옴표로 파일 이름 감싸기
  - 파일 자체를 올리는 것 X. 파일을 생성 또는 변경했다는 행동을 올리는 것.

- ***`git commit`***
  
  : staging area에 있는 파일들을 저장소에 기록. 해당 시점의 버전을 생성하고 변경 이력을 남기는 것. commit의 이름을 메세지라 함.
  
  : commit의 message 중복 가능. commit의 난수가 구분하는 기준. message는 사람을 위한 것이라 중복 가능. 보통 어떤 변경 사항이 있는지 씀.
  
  - *`git commit -m “message”`* : 텍스트 에디터에서 별도의 메시지를 작성하지 않아도 인라인 형식으로 바로 커밋 메시지를 작성
  - *`git commit --amend`* : 마지막 커밋 메시지 수정. 이전 커밋메시지를 수정할 수 있도록 텍스트 에디터가 나타남. 사용하면 안좋음. commit 바꾸면 안 좋음

- ***`git push origin <branch>`***
  
  : staging area에서 add, commit을 마친 파일 및 폴더들을 repository로 전달.
  
  : -u 쓰지 말자. 어느 브랜치에 올릴지 항상 명시하는게 좋음.
  
  : <mark>`git init` 할 필요 없음</mark>(이미 되어있음). 원격 저장소 전체를 복제함.

- ***`git reset HEAD^`***
  
  : Staging Area에 있는 최신 commit 삭제

---

## github에서 가져오려면 할 일

- ***`git pull origin master`***
  
  - `git init`을 한 뒤, github의 내용 가져오기
  1. `git status`로 상태 확인jpu
  
  2. 파일 만들어서 `git init`
  
  3. `git remote add origin <github repository 주소> (ex.[GitHub - imewuzin/TIL: Try!](http://github.com/imewuzin/TIL.git))`
  
  4. 잘못 썼으면 `git remote origin`
  
  5. *`git pull origin master`*
  
      ⇒ repo에 있는거 가져와짐

- ***`git clone`***
  
  : `git init` 할 필요 없음(이미 되어있음). 원격 저장소 전체를 복제함.
  
  1. `*git clone <github repository 주소>* (ex.[GitHub - imewuzin/TIL: Try!](http://github.com/imewuzin/TIL.git))`

- 다른 사람 것도 가져올 수 있음 (조직에 따라 다름)

---

## gitignore

: git에서 특정 파일을 점검하지 못하게 하는 것. ex) 고객 정보

1. .gitignore 파일 생성 (파일명 앞에 '.' 입력, 확장자 없음)

2. a.txt.와 b.txt 파일 생성

3. gitignore에 a.txt 작성

4. `git init` (init 되어 있어도 됨)

5. `git status`
- 프로젝트 초반에 설정함. DB, API 등을 ignore함.

- [gitignore.io - 자신의 프로젝트에 꼭 맞는 .gitignore 파일을 만드세요](https://www.toptal.com/developers/gitignore/)

- 주의사항
  
  - git의 관리를 받은 이력이 있는 파일이나 디렉토리는 나중에 gitignore에 작성해도 적용되지 않음. (git rm --cached 명령어를 통해 git 캐시에서 삭제 필요)

---

## 참고

- Local
  
   : 현재 사용자가 직접 접속하고 있는 기기 또는 시스템. 개인 노트북, 컴퓨터, 태블릿 등 사용자가 직접 조작하는 환경
