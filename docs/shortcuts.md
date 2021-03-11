---
layout: post
title: 자주쓰는 단축키/기능
nav_order: 99
last_modified_date: '2021-03-07 19:00'
---

# **단축키 정리**
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


### Docker 관련 단축키
* `docker ps`: 실행중인 모든 컨테이너 목록보기
* `docker ps -a`: all option
* `docker images`: 실행중인 모든 이미지 목록보기
* `docker rm -f $(docker ps -aq)`: 실행중 & 다운된 도커 모두 삭제 (`-q`: 실행중인 것만 삭제)

* * *

### Git 관련 단축키
#### **상태, 브랜치 관련**
* `git status`: 현재 상태 체크
  * 현재 어떤 branch에 있는지 확인
  * stage/unstage 상태가 어떤지 확인
* `git checkout <branch-name>`: 브랜치 이동
* `git branch <branch-name>`: 브랜치 생성
* `git checkout -b <branch-name>`: 브랜치 생성 & 이동
* `git branch -D <branch-name>`: 브랜치 삭제(local)

#### **커밋, 푸시 관련**
* `git add .`: unstage 상태 👉 stage 상태
  * `git add <폴더이름 or 파일이름> -p`: 부분적으로 stage상태로 바꿈
* `git push origin <branch-name>`: 로컬 저장소 👉원격 저장소
  * `git push -f`: 강제 푸시 (이미 푸시한 커밋을 수정했을 경우 사용)
* `git mv <old path&name> <new path&name>`: 파일 이름/경로 바꾸기
* `git checkout -- .`: 초기상태(unstage된 사항을 모두 되돌림)로 돌아감

#### **취소 관련**
* `git reset --soft HEAD^`: commit을 취소하고, stage상태로 보존
  * `git reset --hard HEAD^`: commit을 취소하고, 변경사항 모두 되돌림
  * `git reset --mixed HEAD^`: commit을 취소하고, unstage상태로 보존
* `git reset --hard <previous commit>`: 해당 커밋으로 되돌아 간다.
* `git clean -f`: untracked file을 모두 삭제
  * `git clean -fd`: 디렉토리까지 모두 지우기

#### **설정 관련**
* `git config --global user.email <이메일주소>`: github에 등록된 이메일주소를 global로 등록
* `git config --global user.name <유저이름>`: github에 등록된 유저이름을 global로 등록
* `git config --list`: config 파일 확인

* * *

# **유용한 기능들**
### 두 커밋을 한 커밋으로 합치고 싶을 때,
1. `git log --oneline`: 로그 확인해서 합치고 싶은 commit버전을 확인하기
2. `git rebase -i <commit-version>`: 해당 버전의 이전 버전으로rebase 진행
3. vi 편집기가 나오면 `i`를 눌러 INSERT모드로 이동
4. 이때, 두 개의 커밋 중 합치고 싶은(삭제하고 싶은) 커밋의 `pick` >>`squash`로 바꾼다.
5. 빠져나온다: esc && `:wq`
6. 그러면 커밋메시지를 수정하라고 나오는데, 왠만하면 주석들 지워주고 원래 커밋의메시지만 남겨둔다. 그리고 다시 esc && `:wq`
7. 이때, 충돌이 일어나는 경우 해당 충돌을 해결해주고 `git add .`해줌(자세한건 rebase로 merge하는 방법 참고)
8. 다 완료되었을 때, `git status`했을 경우 아래와 비슷하게 나옴.
  ```
  and have 1 and 2 different commits each, respectively
  ```
8. `git push -f`

### Rebase로 merge하는 방법
1. `git checkout develop`: 원래 master(혹은 develop) 버전으로체크아웃
2. `git pull`: 원격저장소의 머지사항을 다 반영한다.
3. `git checkout <branch-name>`: 내가 작업한 branch로 이동
4. `git rebase origin/develop`: develop으로 rebase한다.
  이때, Conflict 생길 수 있다. 아래처럼..
  ```
  CONFLICT (content): Merge conflict in client/src/common.ts
  ```
5. 충돌이 생기면, 해당 파일로 가서 수정(충돌 해결)해준다.
6. `git add .`: 충돌난 부분 고친 사항을 add한다.
7. `git rebase --continue`: 다시 rebase를 진행한다.
8. 더이상 충돌이 없을 때까지 계속 이 과정(5->7)을 반복해준다.
9. Applying이 커밋 개수만큼 나오면 완료!
  ```
  Applying Add commit message 1
  Applying Update commit message 2
  ```
10. `git status`: 해당 사항이 잘 반영되었는지 한번 확인해준다.
11. `git push -f`: 강제로 푸시

** 이때, 리베이스 도중에 실수해서 중단하고 싶다면, `git rebase --abort` 실행

* * *

# **Trouble Shooting**
* 갑작스럽게(?) address가 사용중이라고 뜰 때,
  ```
  listen EADDRINUSE: address already in use 127.0.0.1:3000
  ```
  1. `sudo lsof -i :3000`: 3000번을 사용중인 프로세스를 본다.
  2. `kill -9 <PID>`: 그 해당 PID를 죽여준다.