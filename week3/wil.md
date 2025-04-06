# 3주차

## 브랜치 명령어
```bash
# 현재 브랜치 확인 / 모든 브랜치 확인
$ git branch 
$ git branch -a

# 브랜치 생성, 이동
$ git branch <branchName>
$ git checkout <branchName>
$ git checkout -b <branchName>

# 브랜치 삭제
$ git branch -D <branchName>
```

## Pull
```bash
# A브랜치의 변경사항을 현재 브랜치로 가져오기
$ git pull origin A

$ git checkout main
$ git pull origin main
```

## Git Log
- commit 기록을 최신 순으로 확인
- `--oneline`옵션을 사용하면 한 줄에 각 커밋 요약
![alt text](imgs/image.png)

### Commit id
commit 식별을 위해 사용하는 40자 길이의 16진수로, SHA-1 해시 함수를 사용

### HEAD
현재 작업 중인 위치를 가리킴

### git status
세 가지 상태에 따라 파일을 분류하여 확인

## Commit 되돌리기
### commit --amend
- 마지막 commit 내용에 변경이 있을 때 사용
- 완전히 새로운 commit으로 대체되어 commit id도 바뀌게 된다.
- vim으로 진입하여 commit 메시지를 수정하게 된다.

```bash
# 파일 추가 후 커밋에 포함(vim 진입)
$ git add <file>
$ git commit --amend

# -m option으로 vim 진입 없이 commit 메시지 수정
$ git commit --amend -m "commit message"

# --no-edit option으로 commit 메시지 수정 없이 commit 덮어쓰기
$ git commit --amend --no-edit
```
이 때, 다른 사람이 작업 기반으로 삼고 있는 commit은 amend 하면 안된다.

### Reset
commit을 제거하는 데 사용되며 3가지 option이 존재한다.
| 옵션 | index (스테이징 영역) | working directory | 설명|
|------|----------------------|--------------------|------------------------------------------------|
| `--soft`   | ✅ 되돌림               | ❌ 유지             | 커밋만 되돌림 (변경 내용은 그대로 스테이징됨) |
| `--mixed`  | ✅ 되돌림               | ❌ 유지             | 커밋과 스테이징을 되돌림 (변경 내용은 작업 디렉토리에 남음) |
| `--hard`   | ✅ 되돌림               | ✅ 되돌림           | 해당 커밋 상태로 초기화 (되돌릴 수 없음) |
```bash
$ git reset --option <commit Id>

# 사용 예
$ git reset --soft <commit Id>
```

### Revert
> ***코드 상태뿐 아니라 "기록"과 "의도"를 남기기 위해 사용된다.***

주로 아래와 같은 상황에서 사용한다.
- 실수로 올린 커밋을 정식으로 되돌릴 때
- 협업 중에 "이 커밋만 되돌릴게요" 라고 명시할 때
- GitHub PR에서 의도된 롤백을 남기고 싶을 때

#### 예시상황
```mathematica
A - B - C - D
        ↑
       "hello" 추가
```
```mathematica
A - B - C - D - E
                ↑
         "hello" 삭제 (C의 반대)
```

- git revert C의 로그
    ```vbnet
    C: add "hello"
    D: add "world"
    E: revert "add hello"
    ```
- 직접 삭제 후 커밋
    ```sql
    C: add "hello"
    D: add "world"
    E: update a.txt
    ```
    → 의도를 파악하기 어려움

## reset vs revert
- reset은 commit을 삭제하므로 위험
- commit을 공유하는 다른 브랜치에도 영향을 줄 수 있다
- 협업 상황에선 commit을 삭제하기보다 생성하여 되돌리는 revert가 안전하다