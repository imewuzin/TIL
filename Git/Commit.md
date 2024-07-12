# Commit

## 바로 직전 생성한 commit 수정하기

: 불필요한 commit 생성하지 않고, 직전 commit을 수정할 수 있음

- Commit 메시지 수정
  
  ```bash
  git commit --amend
  ```
  
  - commit ID값 변경됨
  
  - 처리 과정
    
    > commit을 Staging area에 올리기 (`git commit -m "message"`) → `git commit --amend` → VIM에서 insert 모드 실행 (i) → commit 수정 → `shift + ;` 명령 모드로 변경 → `wq` write & quit 입력

- Commit 전체 수정
  
  - 같이 commit으로 올렸어야 하는데, 실수로 빼먹었을 때
  
  - 처리 과정
    
    > 빼먹은 commit Staging area에 올리기 (`git commit -m "message"`) → `git commit --amend` → `shift + ;`명령 모드로 변경 →`wq` write & quit 입력
