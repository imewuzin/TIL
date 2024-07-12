# Git revert & reset

## Git revert

: 특정 commit을 없었던 일로 만드는 작업. 변경 사항을 안전하게 실행 취소할 수 있도록 도와주는 <mark>순방향 실행 취소</mark> 작업

```bash
git revert <commit id>
```

- "재설정". 단일 commit을 실행 취소하는 것

- 프로젝트 기록에서 commit을 없었던 일로 처리 후 그 결과를 새로운 commit으로 추가함

- commit 기록에서 commit을 삭제하거나 분리하는 대신, 지정된 변경 사항을 반전시키는 새 commit을 생성

- **처리 과정**
  
  > `git log --one line`  → `git revert <commit id>` id 다 쓸 필요 없이 앞 4자리 정도만 써도 됨  → message 쓰라고 vim editor 켜짐 (첫번째 줄이 Git이 추천한 commit 이름. 왠만하면 그대로 씀) -> `shift + ;` → `wq` 

- **Revert 추가 명령어**
  
  - 공백을 사용해 여러 commit을 한꺼번에 실행 취소 가능
    
    ```bash
    git revert <commit id> <commit id> <commit id>
    ```
  
  - '..'을 사용해 범위를 지정하여 여러 commit을 한꺼번에 실행 취소 가능
    
    ```bash
    git revert <commit id>..<commit id>
    ```
  
  - commit 메시지 작성을 윟나 편집기를 열지 않음 (자동으로 commit 진행)
    
    ```bash
    git revert --no-edit <commit id>
    ```
  
  - 자동으로 commit하지 않고, Staging Area에만 올림 (이후에 직접 commit해야 함). 이 옵션은 여러 commit을 revert할 때 하나의 commit으로 묶는 것이 가능
    
    ```bash
    git revert --no-commit <commit id>
    ```

---

## Git Reset

: reset은 과거 commit으로 되돌아간 후 되돌아간 commit 이후 commit들이 삭제됨.
그런데 삭제되는 commit들의 기록을 어떤 영역에 남겨둘 것인지 옵션을 활용해 조정할 수 있음

```bash
git reset [옵션] <commit id>
```

- **처리 과정**
  
  > `git log --oneline` commit 목록 확인 → `git reset --soft <commit id>` first commit으로 되돌아가기

- **Reset의 3가지 옵션**
  
  - `--soft` : 삭제된 commit의 기록을 staging area에 남김
  
  - `--mixed` : 삭제된 commit의 기록을 working directory에 남김(기본 옵션 값)
  
  - `--hard` :삭제된 commit의 기록을 남기지 않음

---

## 참고

- VIM
  
  : 2가지 모드 존재 (i와 esc로 모드 변경)
  
      1. 입력모드(i) : 키보드로 작성 가능. 하단에 'insert' 떠 있으먀
  
      2. 명령모드(esc) : 저장 등 가능. 마우스로 클릭해도 커서 위치 변화X
  
  - 저장 : `shift + ;` → `wq`

- ```bash
  git reflog
  ```
  
  : HEAD가 이전에 가리켰던 모든 commit을 보여줌
  
  - reset의 `--hard` 옵션을 통해 지워진 commit도 `reflog`로 복구 가능

- ```bash
  git restore
  ```
  
  : Modified 상태의 파일 되돌리기. Working Directory에서 파일을 수정한 뒤, 파일의 수정 사항을 취소하고 원래 파일대로 되돌리는 작업
  
  - 원래 파일로 덮어쓰는 원리이기 때문에 수정한 내용은 전부 사라짐
  
  - `git restore`을 통해 수정 취소 휘 해당 내용을 복원할 수 없음
  
  - 처리 과정
    
    > modified 파일이 있는 상태에서 `git status` → `ret restore <파일명.확장자>`

- Staging area에 올라간 파일을 Unstage 하기
  
  - ```bash
    git rm --cached <파일명.확장자>
    ```
    
    - Staging Area에서 Working Area로 되돌리기
    
    - git 저장소에 commit이 존재하지 않는 경우
  
  - ```bash
    git restore --staged <파일명.확장자>
    ```
    
    - Staging Area에서 Working Area로 되돌리기
    
    - git 저장소에 commit이 존재하는 경우
