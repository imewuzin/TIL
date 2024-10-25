# Git Branch

: 나뭇가지처럼 여러 갈래로 작업 공간을 나누어 독립적으로 작업할 수 있도록 도와주는 Git의 도구

- Branch 장점
  
  - 독립된 개발 환경을 형성하기 때문에 원본(master)에 대해 안전
  
  - 하나의 작업은 하나의 브랜치로 나누어 진행되므로 체계적으로 협업과 개발이 가능
  
  - 손쉽게 브랜치를 생성하고 브랜치 사이를 이동할 수 있음

- Branch를 사용해야 하는 이유
  
  - 브랜치를 통해 별도의 작업 공간을 만들고, 에러가 발생한 버전을 이전 버전으로 되돌리거나 삭제할 수 있음. 브랜치는 완전히 독립되어 있기 때문에 작업 내용이 master 브랜치에 아무 영향을 끼치지 못함. 에러가 해결될 경우, master 브랜치에 반영.

- Master(main) 브랜치의 의미와 역할
  
  - 기본 브랜치(Default Branch)
    
    - 저장소의 초기 상태를 나타내며, 일반적으로 프로젝트의 가장 최신 버전 또는 배포 가능한 안정적인 코드를 포함
  
  - 기준점
    
    - 다른 브랜치가 파생되는 기준점으로 사용됨
  
  - 변경사항 통합

- Branch Command
  
  - **git branch**
    
    | 명령어                       | 기능                     |
    | ------------------------- | ---------------------- |
    | `git branch`              | 브랜치 목록 확인              |
    | `git branch -r`           | 원격 저장소의 브랜치 목록 확인      |
    | `git branch <브랜치 이름>`     | 새로운 브랜치 생성(이동은 X)      |
    | `git branch -d <브랜치 이름>`  | 브랜치 삭제(병합된 브랜치만 삭제 가능) |
    | `git braqnch -D <브랜치 이름>` | 브랜치 삭제(강제 삭제)          |
  
  - **git switch**
    
    : 현재 브랜치에서 다른 브랜치로 `HEAD`를 이동시키는 명령어
    
    | 명령어                                  | 기능                    |
    | ------------------------------------ | --------------------- |
    | `git switch <다른 브랜치 이름>`             | 다른 브랜치로 전환            |
    | `git switch -c <브랜치 이름>`             | 새 브랜치 생성 후 전환         |
    | `git switch -c <브랜치 이름> <commit ID>` | 특정 커밋에서 새 브랜치 생성 후 전환 |
    
    - 주의사항
      - `git add` 하지 않고 브랜치를 이동하면, 이전 브랜치에서 생성한 파일이 이동한 브랜치에도 존재하게 됨
      - Git의 브랜치는 독립적인 작업 공간을 갖지만, Git이 관리하는 파일 트리에 제한됨
      - `git add`를 하지 않았던, 즉 Staging area에 한 번도 올라가지 않은 새 파일은 Git의 버전 관리를 받고 있지 않기 때문에 브랜치가 바뀌더라도 계속 유지
      - 그렇기 때문에 `git switch`를 하지 전, working directory에 있는 파일이 모두 올라갔는지 확인 필요

- `HEAD`
  
  : 현재 브랜치나 commit을 가리키는 포인터(현재 내가 바라보는 위치)

- 사전 준비
  
  1. `mkdir git-branch-practice`
  
  2. `cd git-branch-practice`
  
  3. `code .`
  
  4. `git init`
  
  5. commit 쌓기
     
     1. `touch article.txt`
     
     2. `git add .`
     
     3. 'master-1' 작성
     
     4. `git commit -m "master-1`
     
     5. add 와 commit을 3번 반복
  
  6. 현재 `HEAD`는 `master-3` commit을 바라봄
     
     : `master-1` <- `master-2` <- `master-3` <- `HEAD`

- commit 진행 방향과 화살표 표기 방향이 다른 이유
  
  - commit은 이전 commit 이후에 변경 사항만을 기록한 것
  
  - 즉, 이전 commit에 종속되어 생성됨
  
  - 일반적으로 화살표 방향을 이전 commit을 가리키도록 표기

- Branch 생성 및 조회
  
  1. `git branch login`
  
  2. mater 브랜치에서 commit을 1개 더 작성
     
     - 'master-4' 작성
     
     - `git add .`
     
     - `git comit -m "master-4`
     
     - `git log --oneline`

- Branch 이동
  
  1. 현재 브랜치와 commit의 상태 확인
     
     - `git log --oneline`
     
     - `git switch login` 으로 `login` branch로 이동
       
       : `master-1` <- `master-2` <- `master-3` <- `HEAD`
       
       : `login` -> `master-3`
  - 이제 `HEAD`는 `login` 브랜치를 가르키기 때문에 `git log --oneline` 하면 `master-3` commit까지 보임
  
  - ` git log --oneline --all` 옵션을 이용하면 모든 브랜치의 commit 확인 가능

- Branch에서 Commit 생성하기
  
  1. 새로운 파일 만들기
     - `touch test_login.txt`
  2. commit 생성
     - 'login-1' 작성
     
     - `git add .`
     
     - `git commit -m "login-1"` (`login`)으로 commit되어야 함
  3. `git log --oneline --all --graph`으로 commit의 트리 확인하기

- git branch 정리
  
  - 브랜치의 이동은 HEAD가 특정 브랜치를 가리킨다는 것
  
  - 브랜치는 가장 최신 commit을 가리키므로 HEAD가 해당 브랜치의 최신 commit을 가리킴

---

### Git Merge

: 두 브랜치를 병합

- `git merge <병합 브랜치 이름>`

- master로 이동해서 병합할 브랜치 이름을 가져와야 함!

- 병합 전 확인 및 주의사항
  
  - 수신 브랜치(병합 브랜치를 가져오고자 하는 브랜치) 확인하기
    
    - `git branch` 명령어를 통해 HEAD가 올바른 수신 브랜치를 가리키는지 확인
    
    - 병합 진행 위치는 반드시 수신 브랜치에서 진행되어야 함
  
  - 최신 commit 상태 확인하기
    
    - 수신 브랜치와 병합 브랜치 모두 최신 상태인지 확인

- Merge 종류
  
  - **`Fast-Forward` Merge**
    
    : 브랜치를 '실제로' 병합하는 대신 현재 브랜치 상태를 대상 브랜치 상태로 이동시키는 작업(빨리 감기)
    
    - Merge 과정 없이 단순히 브랜치의 포인터가 앞으로 이동
    
    - ex) `master` 에서 `hotfix`라는 브랜치가 뻗어져 나와 2번의 commit을 했다고 가정할 때, `master`이 가리키는 버전 뒤에 `hotfix`가 가리키는 버전이 있음. `Fast-Forward` Merge를 하면 `hotfix`가 가리키는 버전을 `master`가 가리키게 됨. 즉, `master` HEAD를 최신으로 당겨갈 뿐.
    
    - 실습
      
      1. 사전 준비
         
         - `mkdir fast-forward-practice`
         
         - `cd fast-forward-practice`
         
         - `code .`
         
         - `git init`
         
         - `touch article.txt`
      
      2. commit 생성
         
         - 글 작성 -> `git add .` -> `git commit -m "c1"`
         
         - 글 작성 -> `git add .` -> `git commit -m "c2"`
      
      3. 'c2' commit 기준으로 hotfix 브랜치 생성 : `git branch hotfix`
      
      4. hotfix 브랜치로 이동 : `git switch hotifx`
      
      5. commit 생성
         
         - 글 작성 -> `git add .` -> `git commit -m "c3"`
         
         - 글 작성 -> `git add .` -> `git commit -m "c4"`
      
      6. 병합
         
         - 수신 브랜치로 이동 : `git switch master`
         
         - 병합 진행 : `git merge hotfix`
      
      7. 병합 완료된 브랜치는 삭제 : `git branch -d hotfix` (master)에서 삭제!
  
  - **`3-Way` Merge**
    
    : 병합하는 각 브랜치의 commit 2개와 공통조상 commit 하나를 사용하여 병합하는 작업 (총 3개의 commit 사용) 
    
    - 실습
      
      1. 사전 준비
         
         - `mkdir 3-way-merge-practice`
         
         - `cd 3-way-merge-practice`
         
         - `code .`
         
         - `git init`
         
         - `touch article.txt`
      
      2. commit 생성
         
         - 글 작성 -> `git add .` -> `git commit -m "c1"`
         
         - 글 작성 ->`git add .` -> `git commit -m "c2"`
      
      3. hotfix 생성 및 이동 `git switch -c hotfix`
         
         : `hotfix`HEAD -> `c2` <- `master`
      
      4. 변경 사항, commit 생성
         
         - `touch hotfix-article.txt`
         
         - 글 작성 -> `git add .` -> `git commit -m "c3"`
         
         - 글 작성 -> `git add .` -> `git commit -m "c4"`
      
      5. 병합 전 브랜치 하나 더 생성하기
         
         - master 브랜치로 이동 : `git switch master`
         
         - article.txt에 master-3 작성 후 commit 생성 :`git add .` -> `git commit -m "c5"`
         
         - `c1` <- `c2` <- `c3` <- `c4` <- `hotfix` <- `Merge branch 'hotfix'`
           
           - `c2` <- `c5` <- `master`  <- `Merge branch 'hotfix`'
      
      6. 병합
         
         - 수신 브랜치 : master, 병합 브랜치 : hotfix
         
         - `git merge hotfix`
         
         - 'bin additor' 창 뜸
           
           - `c4`와 `c5`를 병합하려 하는데, commit의 이름을 물어보기 위해
           
           - `esc` 누르면 명령어 입력 가능
           
           - `: wq` : 저장 후 나가기 -> enter

- **Merge Conflict**
  
  : 병합하려는 두 브랜치가 '동일한 파일의 동일한 부분'에서 변경된 후 병합 시 충돌이 발생하는 것
  
  - 3-way-merge 에서만 발생
  
  - git이 충돌을 표시하는 방식
    
    - 충돌 마커로 확인 가능
    
    - `<<<<<<` HEAD : 현재 브런치
    
    - `=======` : 아래 내용이 합치려는 브랜치의 내용
    
    - `>>>>>>` merge branch : 병합하려는 브랜치
    
    - 수정하려면 마커도 지워야 함
  
  - git 충돌을 해결하는 과정
    
    1. 충돌하는 부분을 확인한 후에는 원하는대로 충돌 내용ㅇ르 수정
    
    2. 병합을 완료할 준비가 되면 충돌하는 파일에서 `git add` 명령을 실행
    
    3. 그런 다음 `git commit`을 실행하여 `merge commit`을 생성
  
  - 실습 - 3-way-merge에 이어서
    
    1. master에서 commit 2개 생성
       
       - `touch ssafy.txt`
       
       - 글 작성 -> `git add .` -> `git commit -m "c1"`
       
       - 글 작성 -> `git add .` -> `git commit -m "c2"`
    
    2. hotfix 생성
       
       - hotfix 지우기 : `git branch -d hotfix` (이전 것이 남아있어서 지운 뒤 재생성)
       
       - hotfix 생성 : `git branch hotfix`
    
    3. master에서 commit 2개 더 생성
       
       - 글 작성 -> `git add .` -> `git commit -m "c3"`
       
       - 글 작성 -> `git add .` -> `git commit -m "c4"`
    
    4. hotfix로 이동 : `git switch hotfix`
    
    5. ssafy.txt 파일 3번째 라인에 hotfix-1 작성 -> `git add .` -> `git commit -m "c3"`
    
    6. master로 이동 : `git switch master`
    
    7. 병합 (충돌 발생) : `git merge hotfix`
    
    8. 마커 참고해 수정
    
    9. 변경사항 다시 올리기 : `git add .` -> `git commit`
       
       - `(master|MERGING)`으로 병합 진행중임을 알 수 있음
    
    10. `bin additor` 창 뜸 -> `:wq` 입력
    
    11. 병합 완료된 브랜치 삭제 : `git branch -d hotfix`
    
    12. `git log --oneline --graph` 로 확인 가능



---

## Git Workflow

: 원격 저장소를 활용해 다른 사용자와 협업하는 방법

- **Feature Branch Workflow**
  
  : Shared repository model (각 사용자가 원격저장소의 소유권을 공유받는 방식)
  
  - 실습
    
    1. 사전 준비
       
       - github에서 repository 생성
       
       . 팀원 초대 : Settings -> Collaborators -> Add People
       
       . 팀원 이메일에서 초대장 수락
    
    2. 각 사용자는 원격 저장소의 소유권을 가진 상태. 따라서 `clone`을 통해 저장소를 로컬에 복제
    
    3. 기능 추가를 위해 branch 생성 및 기능 구현
       
       - ex) `git branch feature/login` -> `git switch feature/login`
       
       - ex) `git switch -c feature/login`
    
    4. 기능 구현 후 원격 저장소에 브랜치 반영
       
       - master이 아니라 내가 작업한 브랜치로 push해야 함!
       
       - `git add .` -> `git commit -m "기능 구현"` -> `git push origin feature/login`
    
    5. PR (pull request) - 팀장
       
       - 팀장의 repository에 노란 창 뜲
         
         - 안 뜰 경우, Pull requests에서 확인 가능
       . 노란 창의 `Compare & pull request` 클릭
       
       . 팀장 화면 오른쪽에 Reviews, Assigness 등 있음. 추가 옵션 설정 가능
       
       . `Create pull request` 클릭
       
       . Add a comment 창에서 댓글 달며 코드 수정 가능
    
    6. PR (pull request) - 팀원
       
       - 팀장의 repository로 들어가면 노란창 뜲 (or Pull requests)
       
       - `Compare & pull request` -> `Create pull request`
    
    7. 팀장이 `Merge Pull requests` 버튼으로 병합 가능. 거절은 `Close pull requests`
    
    8. 팀원들 master 브랜치로 이동 : `git switch master`
    
    9. 병합된 버전 가져오기 : `git pull origin master`
    
    10. 병합 완료된 로컬 브랜치 삭제 : `git branch -d feature/login`
        
        - github에서도 브랜치 삭제해야 함

- **Forking Workflow**
  
  : Fork & Pull model (각 사용자가 소유권이 없는 원격저장소를 복제하는 방식)
  
  - Fork
    
    : 다른 사용자의 원격 저장소를 자신의 계정으로 복사하는 것
    
    - 특징
      
      - 원본 저장소와 연결을 유지하면서 독립적인 복사본을 만듦
      
      - 우너본 저장소에 영향을 주지 않고 변경 사항을 만들 수 있음
  
  - Fork 작업 흐름
    
    1. 소유권이 없는 원격 저장소를 fork를 통해 복제
       
       - 복제된 저장소는 작게 `forked from ~~` 써 있음
    
    2. 복제된 저장소를 clone 받기
    
    3. 추후 로컬 저장소를 원본 원격 저장소와 동기화하기 위해 URL을 연결
       
       - `git remote add upstream [원본url]`
       
       - upstream은 예시. origin이 아닌 다른 이름으로 작성할 것
    
    4. 브랜치 생성 -> 기능 구현
    
    5. 복제 원격 저장소에 브랜치 반영 -> PR 만들기
    
    6. 완성된 PR을 원본 원격 저장소에 Pull Request
    
    7. 원본이 최신 버전이니, master 브랜치로 switch -> `git pull upstream master`
    
    8. 병합 완료된 브랜치 삭제 (github 브랜치도)


