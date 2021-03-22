---
layout: post
title: Git & GitHub 기초
nav_order: 87
last_modified_date: 2021-03-22 21:37
lastmod: 2021-03-22 21:37
---

# **Git & GitHub 기초**
## What is Git/GitHub ?
* Git
  * 분산 버전 관리 시스템
  * 목적: 파일의 변경사항을 지속적으로 추적하기 위함
* Github
  * 분산 버전 관리 툴(Git)을 사용하는 프로젝트를 지원하는 <span class='text-purple-000'>웹호스팅 서비스</span>
  * 깃을 사용하여 웹에 저장할 수 있도록 도와준다. (Local Repository & Remote Repository)
  * 다른 서비스로는 GitLab도 있다.
* git이 프로그래밍 언어라면, github는 그 언어를 사용할 수 있게 해주는 tool 또는 IDE 같은 느낌이라고 생각할 수 있다. (사실 조금 개념이 다르지만, 초보자에겐 이렇게 생각하는게 좀더 쉽기 때문에 이렇게 설명한다. 더 정확한 차이점은 이 글을 읽다보면, 그리고 다루다보면 그 차이를 알 수 있다.)

* * *

## Git 기초
### Branch 종류
1. Master(Main)
* 가장 기본적인 브랜치
* 이미 <span class='text-purple-000'>배포가 완료된 안정적인 버전</span>이다.
* (이상적으로는) 버그가 없다.

2. Develop
* <span class='text-purple-000'>개발이 진행중</span>인 또 다른 기본 브랜치
* 개발자들이 각자 개발(새로운 기능, refactoring, bug fix 등)을 완료하고 Merge하는 기준 브랜치이다.
* 안정적인 버전이 아니기 때문에, 버그가 (많이)있다.

3. Release
* <span class='text-purple-000'>배포를 준비</span>하는 브랜치
* 개발이 마무리가 되어 더이상 새로운 기능을 개발하지 않고, develop 브랜치로부터 release 브랜치를 생성한다.
* 이 브랜치에서는 새로운 기능을 개발하지 않는다.
* 버그만 고쳐서 이 브랜치에 Merge한다.

4. Feature
* 특정 (큰)기능을 개발하기 위해 생성하는 브랜치
* Develop 브랜치로부터 생성되는 브랜치이며, 큰 기능을 구현하기 위한 브랜치이다.
* 이 브랜치를 기준으로 실제 작업이 이루어지는 작은 브랜치를 생성해서 개발한다.
* 작은 브랜치들은 feature 브랜치로 Merge되며, 해당 기능이 개발 완료되면 Develop 브랜치에 Merge된다.

5. Hotfix
* Master에서 생긴 버그를 고치는 브랜치
* 버그를 고친 후에 Master와 Develop에 둘다 Merge한다.


### Unstage/Stage 상태
Commit을 만들기 위해 상태를 변경해줘야 한다. 그러기 위해서 해당 상태를 알아야 한다.
* `git add <filepath>`: Unstage/Untracked 👉 Stage
* `git commit -m "Add commit message"`: Staged Files 👉 1 commit
* `git push origin <branch name>`: commits 👉 Pull Request (Remote Repository)

이런식으로 add, commit, push의 과정을 거치게 되는데, 
* add는 파일 하나하나를 stage 상태로 만드는 것이다. 여기서 stage상태들이 모여서 1commit이 된다.
* 1 commit은 작은 작업단위라고 생각하면 된다.
* 여러개의 커밋들이 모여서 브랜치의 작업 내용이 되는 것이다. 이를 Remote Repository에 반영하기 위해 push한다.
* 예를 들어 이런식으로 생각할 수 있다.
    ```
    Branch            Commit                  Stage
    13-user-feature - Create user function  ⏉ createUserController.js
                                            ⎿ createUserService.js
                    - Select user function  ⏉ selectUserController.js
                                            ⎿ selectUserService.js
    ```
* 사실 git은 변경사항을 한 줄씩 저장하기 때문에, stage 상태로 만드는 것은 파일단위가 아니라 한 파일에서도 부분적으로 추가해줄 수도 있다.
  ```
  git add -p <filepath>
  ```

반대로 Commit을 실수로 했다치자, 어떻게 반대의 상태로 바꿀 수 있을까?
* `git reset --soft HEAD^`: 1개의 최근 commit을 stage상태로 되돌린다.
* `git reset --mixed HEAD^`: 1개의 최근 commit을 unstage상태로 되돌린다.
* `git reset --hard HEAD^`: 1개의 최근 commit을 drop시킨다.
* `git reset HEAD <filepath>`: stage된 내용을 취소한다.
* `git clean -f`: untracked file을 모두 삭제한다.


### Merge/Rebase
1. Merge
내가 작업한 내용의 branch를 기준 브랜치로 합치는 작업이다.
그런데 이때 그냥 Merge하게 되면, 내 작업한 내용과 이미 기준브랜치에 그동안 쌓인 commit내용 때문에 git의 flow가 복잡해진다.
기준 브랜치에서 새로운 브랜치를 생성하게 된 기준점이 있을 것이다. 그 기준점을 기준 브랜치의 최신 commit 지점으로 옮겨줘야 한다. 그것을 Rebase라고 한다.

2. Rebase
위에서 말한 기준점을 옮기는 Rebase가 있고, 내가 작업한 브랜치에서 commit을 합치는 등의 작업을 하기위한 Rebase도 있다.
    * `git rebase origin/develop`: 내가 작업하는 브랜치의 기준점을 develop 브랜치의 최신지점으로 옮겨주는 작업이다. 이때 충돌이 생길 수 있다. ([해결법](https://jjuhey.github.io/docs/shortcuts/#rebase%EB%A1%9C-merge%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95))
    * `git rebase -i <commit version>`: 내가 작업한 브랜치에서 두 커밋을 하나로 합치고 싶을 때, 이때도 충돌이 생길 수 있다.([해결법](https://jjuhey.github.io/docs/shortcuts/#%EB%91%90-%EC%BB%A4%EB%B0%8B%EC%9D%84-%ED%95%9C-%EC%BB%A4%EB%B0%8B%EC%9C%BC%EB%A1%9C-%ED%95%A9%EC%B9%98%EA%B3%A0-%EC%8B%B6%EC%9D%84-%EB%95%8C))

* * *

## Github 기초
### Repository
1. Local Repository
내 컴퓨터에 있는 저장소를 의미한다. 깃허브에 있는 원격저장소(remote repository)에서 복사해올 수 있다.
    * Remote 👉 Local
        * `git clone git@github.com:<username>/<github-repo>.git`: 복사해오기, [SSH 이용하기](https://jjuhey.github.io/docs/set-ssh-github/)
        * `git pull`: 원격저장소의 반영된 내용을 내 로컬저장소에도 반영하기 (Download + Merge)
        * `git fetch`

2. Remote Repository
깃허브에 저장되어 있는 저장소를 의미한다. 내 로컬저장소(local repository)의 작업내용을 원격 저장소에 반영할 수 있다.
    * Local 👉 Remote
        * `git push origin <branch-name>`: 해당 브랜치에 내가 생성한 커밋을 원격저장소에 반영하기
        * `git push -f`: 내 커밋과 원격저장소 커밋이 다를 때, 강제로 내 로컬 커밋을 반영해줌(rebase기능을 사용했을 때 이 방식으로 푸시한다. 단, 원격저장소에 강제 반영이라 조심!)

### Issue
* 기능 개발, 문서작업, 버그 리포트, 개선 사항 등 다양한 사항을 기록할 수 있다. 말 그대로 내 저장소에 관련된 모든 '이슈'에 대한 내용들을 적는 곳이다.
* 이슈 번호(#1, #2, ...)로 branch 이름을 지을 수 있다. : `13-user-feature`

### Pull Request
* Merge를 위해 하나의 Pull Request(이하 PR)에 하나의 branch를 매칭시킬 수 있다.
* 커밋들을 push하면, github에 PR을 생성하라는 경고(?) 문구가 뜬다. 거기서 생성해주면 된다.
* 여기에서 Linked Issue를 연결(Closes #13: Merge되면 그 이슈가 닫히도록 한다.)한다.
* Review 요청을 통해 여기서 리뷰를 받을 수 있다. (LGTM~!)

### Github로 협업하기
* (Github) Issue 생성 👉
* (My Computer) 👉 branch 생성 👉 개발/작업 👉 Commit 👉 Push 👉
* (Github)👉 PR 생성 👉 Review 요청 👉 Feedback 반영(+Resolve) 👉 Merge 완료