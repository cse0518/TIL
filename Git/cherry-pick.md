# cherry-pick

- 다른 브랜치의 commit을 그대로 옮겨오고 싶은 경우
  ```bash
  git cherry-pick commit_hashcode
  ```
  - 여러개의 commit을 cherry-pick 하려면
    ```bash
    git cherry-pick commit_hashcode1 commit_hashcode2 commit_hashcode3 ...
    ```
  - cherry-pick 이후에도 계속 cherry-pick 상태로 있어서 push 해줘야 함.
  - merge가 안된 파일이 있다고 충돌이 나면 git pull을 해오던가 버전을 맞추고 다시 cherry-pick 해야됨.
<br/>

- cherry-pick 상태에서 또 cherry-pick을 시도하면 충돌남.  
  cherry-pick 상태에서 나오고 다시 해줘야함.
  - cherry-pick --continue
    - 충돌을 해결하고 cherry-pick 다시 진행
      ```bash
      git cherry-pick --continue
      ```
  - cherry-pick --abort
    - cherry-pick을 중단함
      ```bash
      git cherry-pick --abort
      ```
___