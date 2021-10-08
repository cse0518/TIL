# reset
- reset
  - stage를 reset 한다.
  - git add 한 파일들이 unstaged 영역으로 간다.
    ```bash
    git reset
    ```
- reset 특정_commit
  - 특정 commit으로 돌아간다.
  - 로컬 소스코드는 그대로이다.
    ```bash
    git reset commit_hashcode
    ```
- soft reset
  - 특정 commit으로 돌아간다.
  - 로컬 소스코드도 특정 commit으로 돌아간다.
  - 특정 commit 이후의 commit 기록이 전부 삭제된다.
  - 단, 변경 사항을 stage 영역에 남겨놓는다.  
    commit 하면 되돌리기 전으로 돌아간다.
    ```bash
    git reset --soft commit_hashcode
    ```
- hard reset
  - 특정 commit으로 돌아간다.
  - 로컬 소스코드도 특정 commit으로 돌아간다.
  - 특정 commit 이후의 commit 기록이 전부 삭제된다.
    ```bash
    git reset --hard commit_hashcode
    ```

# revert
- revert
  - 이전 commit 버전으로 가고싶을때
  - 이전 commit이 남아있고 예전 버전이 최신 버전으로 추가된다.
    ```bash
    git revert commit_hashcode
    ```
___