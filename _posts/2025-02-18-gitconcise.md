---
layout: post
title: "Git concise"
date: 2025-2-18
categories: [git]
tags: [git]
---

# git concise

1. 기본 작업

| 명령어 | 설명 |
| --- | --- |
| git init | 새로운 Git 저장소를 생성합니다. |
| git clone <repository_url> | 원격 저장소를 복제하여 로컬에 저장소를 생성합니다. |
| git status | 현재 브랜치 상태(추적되지 않은 파일, 변경 내용 등)를 보여줍니다. |
| git add <file> | 특정 파일을 스테이징 영역에 추가합니다. |
| git add . | 현재 디렉토리 내 모든 파일을 스테이징 영역에 추가합니다. |
| git commit -m "message" | 스테이징된 변경 사항을 커밋합니다. |
| git commit --amend | 마지막 커밋 메시지를 수정하거나 변경 사항을 추가합니다. |
| git log | 커밋 기록을 확인합니다. |

2. 브랜치 관리

| 명령어 | 설명 |
| --- | --- |
| git branch | 로컬 브랜치 목록을 확인합니다. |
| git branch <branch_name> | 새로운 브랜치를 생성합니다. |
| git checkout <branch_name> | 특정 브랜치로 전환합니다. |
| git checkout -b <branch_name> | 새로운 브랜치를 생성하고 해당 브랜치로 전환합니다. |
| git merge <branch_name> | 현재 브랜치에 다른 브랜치를 병합합니다. |
| git branch -d <branch_name> | 로컬 브랜치를 삭제합니다. |
| git branch -r | 원격 브랜치 목록을 확인합니다. |
| git push origin --delete <branch_name> | 원격 브랜치를 삭제합니다. |

3. 원격 저장소

| 명령어 | 설명 |
| --- | --- |
| git remote -v | 원격 저장소 목록을 확인합니다. |
| git remote add <name> <url> | 새로운 원격 저장소를 추가합니다. |
| git fetch | 원격 저장소에서 데이터를 가져옵니다(병합하지 않음). |
| git pull | 원격 저장소에서 데이터를 가져오고 병합합니다. |
| git push | 로컬 브랜치의 변경 사항을 원격 저장소에 푸시합니다. |
| git push -u origin <branch_name> | 로컬 브랜치를 원격 브랜치에 연결하고 푸시합니다. |

4. 리셋 및 되돌리기

| 명령어 | 설명 |
| --- | --- |
| git reset --soft <commit_hash> | 특정 커밋으로 이동하며 변경 내용은 유지합니다(스테이징 상태). |
| git reset --mixed <commit_hash> | 특정 커밋으로 이동하며 변경 내용은 유지하되, 스테이징은 해제합니다. |
| git reset --hard <commit_hash> | 특정 커밋으로 이동하며 변경 내용과 스테이징 모두 삭제합니다. |
| git revert <commit_hash> | 특정 커밋을 되돌리는 새로운 커밋을 생성합니다. |
| git checkout <commit_hash> | 특정 커밋 상태를 임시로 확인합니다. |

5. 태그(Tag)

| 명령어 | 설명 |
| --- | --- |
| git tag | 태그 목록을 확인합니다. |
| git tag <tag_name> | 태그를 생성합니다. |
| git tag -a <tag_name> -m "msg" | 주석이 포함된 태그를 생성합니다. |
| git push origin <tag_name> | 특정 태그를 원격 저장소에 푸시합니다. |
| git push origin --tags | 모든 태그를 원격 저장소에 푸시합니다. |
| git tag -d <tag_name> | 로컬 태그를 삭제합니다. |
| git push origin --delete <tag> | 원격 저장소에서 태그를 삭제합니다. |

6. 기타 유용한 명령

| 명령어 | 설명 |
| --- | --- |
| git stash | 현재 작업 내용을 임시 저장하고, 워킹 디렉토리를 깨끗하게 만듭니다. |
| git stash list | 저장된 stash 목록을 확인합니다. |
| git stash apply | 마지막 stash를 적용합니다(내용은 그대로 유지). |
| git stash pop | 마지막 stash를 적용하고, stash에서 제거합니다. |
| git diff | 변경된 내용을 확인합니다. |
| git diff <branch1>..<branch2> | 두 브랜치 간의 차이를 확인합니다. |
| git blame <file> | 파일의 각 라인에 대한 변경 내역을 확인합니다. |

7. 상황별 명령 요약

| 상황 | 필요한 명령어 |
| --- | --- |
| 원격 브랜치 복제 | git clone <repository_url> |
| 새로운 브랜치 생성 후 전환 | git checkout -b <branch_name> |
| 커밋 메시지 수정 | git commit --amend |
| 병합 충돌 해결 | 1. 충돌 수정 → 2. git add <file> → 3. git commit |
| 푸시 강제 수행 | git push --force |
| 파일 복구 | git checkout HEAD <file> |

**Git에서 Staging의 개념**

- *Staging(스테이징)**이란 Git에서 커밋하기 전 준비 작업을 하는 단계를 의미합니다. 변경된 파일들을 커밋에 포함하기 위해 선택적으로 추가하는 공간을 Staging Area(스테이징 영역) 또는 **Index(인덱스)**라고 합니다.

**Git에서의 작업 흐름**

- Working Directory(작업 디렉토리)
    
    파일을 수정하거나 새로 생성하는 단계.
    
    아직 Git이 관리하지 않거나 추적 중인 파일들이 이 상태에 있습니다.
    
- Staging Area(스테이징 영역)
    
    커밋할 준비가 된 파일들이 위치하는 임시 공간.
    
    특정 파일 또는 모든 파일을 스테이징 영역에 추가할 수 있습니다.
    
- Repository(저장소)
    
    커밋된 변경사항이 저장되는 영역입니다.
    
    커밋 후에는 변경 기록이 영구히 저장소에 기록됩니다.
    

**Staging과 관련된 명령어**

1. 파일을 Staging Area에 추가
    
    `git add <file>`
    
    특정 파일을 스테이징 영역에 추가합니다.
    
    예: index.html 파일 추가
    
    `git add index.html`
    

2. 디렉토리를 Staging Area에 추가
    
    `git add <directory>`
    
    디렉토리와 하위 파일 전체를 스테이징 영역에 추가합니다.
    
    예: 현재 디렉토리 전체 추가
    
    `git add .`
    

3. 변경된 파일 확인
    
    `git status`
    
    Unstaged changes: 스테이징 영역에 추가되지 않은 변경 사항.
    
    Changes to be committed: 스테이징 영역에 추가된 변경 사항.
    

4. Staging Area에 있는 변경사항 확인
    
    `git diff --cached`
    
    스테이징된 파일의 변경 내용을 확인합니다.
    

5. Staging Area에서 파일 제거
    
    `git reset <file>`
    
    특정 파일을 스테이징 영역에서 제거하고 작업 디렉토리 상태로 되돌립니다.
    

stash와 차이점

| 구분 | Staging | Stash |
| --- | --- | --- |
| 주요 목적 | 변경 사항을 선택적으로 커밋하기 전 준비 상태로 만듦. | 현재 작업을 임시로 저장하여 다른 작업을 수행할 수 있게 함. |
| 저장 위치 | Staging Area(스테이징 영역) | 임시 스택 구조의 공간. |
| 변경 사항 유지 여부 | 작업 디렉토리에 변경 사항이 남아 있음. | 작업 디렉토리가 초기화되어 변경 사항이 제거됨. |
| 저장 단위 | 선택한 파일만 스테이징 가능. | 전체 변경 사항을 저장하거나, 옵션을 통해 확장 가능. |
| 복원 방식 | 스테이징된 상태를 유지하며 커밋 또는 리셋 가능. | stash apply나 stash pop으로 복원. |
| 파일 추가 가능 여부 | 원하는 파일만 추가 가능. | 변경 사항 전체를 저장하거나, 추적되지 않은 파일도 포함 가능. |
| 사용 사례 | 커밋에 포함할 파일을 선별하고 관리. | 작업 중단 후 다른 브랜치 전환 또는 긴급 작업 수행. |
| 명령어 | git add | git stash |


- **Remote repository에 push한 커밋을 되돌리는 방법**

1. **잘못된 커밋을 삭제하고 이전 상태로 되돌리기**
    
    이 방법은 이전 상태로 완전히 되돌리고, remote에서도 해당 커밋을 삭제하는 방식입니다.
    
    1) 이전 커밋으로 이동
    
    `git reset --hard <이전 커밋의 해시>`
    
    2) 강제 push로 remote repository 동기화
    
    `git push origin <branch-name> --force`
    
    `git push origin main --force`
    
    - 주의
    
    이 방법은 force push를 사용하므로, 협업 중인 다른 개발자들이 영향을 받을 수 있습니다. 주의 깊게 사용하세요.
    
    -force를 사용할 경우, remote에 있는 변경 사항이 덮어씌워지니 신중해야 합니다.
    

2. **Revert로 커밋 취소**
    
    이 방법은 새로운 커밋을 만들어 기존 커밋을 취소하는 방식입니다. 협업 중일 때 추천됩니다.
    
    1) 특정 커밋을 취소
    
    `git revert <커밋 해시>`
    
    2) 변경 사항을 remote에 push
    
    `git push origin <branch-name>`
    
    - 특징
    
    기존 커밋 이력은 유지되며, 취소를 위한 새로운 커밋이 생성됩니다.
    
    다른 개발자와 협업 중일 때 안전한 방식입니다.
    

3. **특정 파일만 되돌리기**
    
    전체 커밋이 아니라 특정 파일만 이전 상태로 되돌리고 싶을 때 사용합니다.
    
    1) 특정 파일을 이전 커밋 상태로 복원
    
    `git checkout <이전 커밋 해시> -- <파일명>`
    
    2) 복원된 파일을 커밋
    
    `git commit -m "Revert changes in <파일명>"`
    
    3) remote에 push
    
    `git push origin <branch-name>`
    

1. **Reset 후 커밋 취소**
    
    push한 커밋을 되돌리고 커밋 기록을 아예 삭제하고 싶을 때 사용합니다.
    
    1) 이전 커밋으로 soft reset
    
    `git reset --soft <이전 커밋의 해시>`
    
    2) 변경 사항을 stage에 올림
    
    `git add .`
    
    3) 새로운 커밋 생성
    
    `git commit -m "Undo previous commits"`
    
    4) remote에 push
    
    `git push origin <branch-name>`
    


**어떤 방법을 선택해야 할까요?**

- 혼자 작업 중: git reset과 --force 사용 가능.
- 협업 중: git revert 사용 권장.
- 특정 파일만 복구: git checkout 사용.

git 완전초기화

`rm -rf .git`

현재 클론 repository의 원격 저장소확인

`git remote -v`

사용자 이메일, 이름 설정

`git config --global user.name "GsLee"`

`git config --global user.email [lgswin@gmail.com](mailto:lgswin@gmail.com)`

git에 아직 싱크되지 않은 commit이 있는 상태에서 local에 새로운 commit을 푸시하려고 할때 서로 충돌 나는 상태

1.	Fetch the latest changes from the remote repository:

`git fetch origin`

2.	Merge the remote changes into your local branch:

`git merge origin/main`

3.	If everything is fine, commit the merge:

`git commit -m "Merged origin/main into main"`

4.	Push the merged changes back to the remote repository:

`git push origin main`