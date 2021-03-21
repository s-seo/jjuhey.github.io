---
layout: post
title: SSH & 계정별 설정
nav_order: 88
last_modified_date: 2021-03-21 13:54
lastmod: 2021-03-21 13:54
---

# **SSH 설정하기 & 계정 두개 사용하기**

## SSH키 생성하기
1. `~/.ssh`: 터미널을 켜고 이동한다.
    * 이때, 해당 경로가 없다면 만들어줌: `mkdir ~/.ssh`
2. `chmod 700 ~/.ssh`
3. `ssh-keygen -t rsa -b 4096 -C <email주소>`
    * github계정의 email주소로 ssh를 생성해준다.
4. 파일 이름을 바꿀것인지 물어보는데, 처음 만드는 것이라면 default(id_rsa)로 그냥 만든다.
5. 패스워드 입력
6. `ls -l`: 키 파일(id_rsa, id_rsa.pub)이 생성되었는지 확인한다.
* * * 

## 공개키 등록하기
1. `cat ~/.ssh/id_rsa.pub`: 공개키 내용을 확인한다. 그리고 복사한다.
    * 복사만 하고싶다면, `pbcopy < ~/.ssh/id_rsa.pub`
2. github 홈페이지로 가서 Settings > SSH and GPG keys > New SSH key
    <img width="700" alt="ssh keys" src="https://user-images.githubusercontent.com/53938072/111894475-61556280-8a4e-11eb-99fc-1c5401bbd8b0.png">
3. 우측의 New SSH key를 눌러서 Key라고 있는 부분에 복사한 공개키를 붙여넣는다.
4. Title은 알아서 잘 입력한다.
* * *

## 계정별 설정하기
개정을 두개 사용, 필자같은 경우 회사 github계정과 개인 계정이 다르기 때문에 각각 사용할 수 있는 SSH를 따로 생성하였다. 회사 컴퓨터에는 이미 `id_rsa`가 회사 계정에 등록되어 있고, 이제 개인 계정도 등록해보자.
1. SSH키를 다시 생성해야 하는데, `.ssh`경로로 이동하여 `SSH키 생성하기` 파트의 3 ~ 5번을 똑같이 해준다.
    * 이 때, 4번의 파일이름을 다르게 설정해야 하는데, 필자같은 경우에 `id_rsa_personal2` 이런식으로 생성해주었다. 비밀번호는 까먹을 수 있어서, 동일하게 해줌!
    <img width="566" alt="스크린샷 2021-03-21 오후 2 12 20" src="https://user-images.githubusercontent.com/53938072/111894634-decda280-8a4f-11eb-9c1b-b2c83f8413a2.png">
2. `touch config`: config 파일을 만든다.
3. `vim config`: 파일 수정
    ```shell
    # account: company
    Host github.com
	  HostName github.com
	  User jjenna
	  IdentityFile ~/.ssh/id_rsa

    # account: personal
    Host github.com-personal
	  HostName github.com
	  User JJuhey
	  IdentityFile ~/.ssh/id_rsa_personal2
    ```
    * 여기서 중요한 부분은 Host와 IdentityFile이다. git clone할 때, Host를 가지고 그에 해당하는 키를 찾게된다.
4. git clone시에 특정 ssh key를 사용하도록 설정
    ```shell
    git clone git@github.com-personal:repo_user/repo_name.git
    ```
    * 이런식으로 `github.com`부분을 (Host로 설정했던)`github.com-personal`로 바꿔서 입력해준다.
    * 그러면 해당 레포를 이용할 때, SSH키로 `id_rsa_personal2`를 사용하게 된다!
