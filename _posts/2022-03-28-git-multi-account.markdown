---
layout: post
title:  "[git] How to use multi accounts in git?"
date:   2022-03-28
category: git
---

## Intro.
나는 github 계정이 개인 용도와 회사 용도 두 개 있다. 내 작업물을 push할 때 마다 올바른 계정 정보가 저장돼 있지 않아서 권한 문제로 거부 될 때가 많았다. 여러 계정을 한 번에 사용할 수 없을까?

`undol26A`, `undol26B` 두 개의 계정을 사용한다고 가정하자.

## ssh-key 생성

```bash
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C "undol26A@gmail.com"
```

<br>
이 때 **처음 key를 저장할 파일 명이 중요**하다. (`id_rsa_undol26A`)

```bash
# Generating public/private rsa key pair.
# Enter file in which to save the key (/Users/blabla/.ssh/id_rsa): id_rsa_undol26A
```

undol26B도 같은 식으로 만들면 된다.

## github에 ssh 등록
<center>
<figure>
	<img src="/public/img/git/ssh01.jpg" alt="" width="23%" height="23%"> 
  <img src="/public/img/git/ssh02.jpg" alt="" width="23%" height="23%"> 
  <img src="/public/img/git/ssh03.jpg" alt="" width="23%" height="23%"> 
  <img src="/public/img/git/ssh04.jpg" alt="" width="23%" height="23%"> 
</figure>
</center>

순서대로 따라하면 된다. 

github에 로그인한 자신의 계정에서 차례로 `1. 계정 선택` -> `2 Settings` -> `3. SSH and GPG keys` -> `4. New SSH key` 를 누르면 
`5. Title` 에 자신이 구분할 ssh 이름을 원하는 대로 짓고 (`ex. undol26-personal`) `6. key`에 위 1번에서 생성한 public key 를 붙여 넣는다. `7. Add SSH Key`를 눌러 마무리 한다.

public key는 아래 명령어를 통해 알 수 있다.

```bash
cat ~/.ssh/id_rsa_undol26A.pub
```

github에 undol26A 계정으로 로그인 한 뒤, 위의 ssh-key를 복사하고 다시 github에 undol26B 계정으로 로그인 한 뒤, undol26B에 해당하는 ssh-key를 다시 복사한다.

## config 파일 생성
```bash
vim ~/.ssh/config
```

아래와 같이 생성한다.
```bash
# undol26A
Host github-undol26A
        HostName github.com
        User git
        IdentityFile /home/undol26/.ssh/id_rsa_undol26A

# undol26B
Host github-undol26B
        HostName github.com
        User git
        IdentityFile /home/undol26/.ssh/id_rsa_undol26B
```

- Host: 내가 정하는 이름이다. (마음대로 정해도 된다.)
- HostName, User: 똑같이
- IdentityFile: 앞에는 본인의 경로를 적어주고(`/home/undol26/.ssh/`) 마지막 `id_rsa_undol26A`을 위의 **ssh-key 생성에서 저장한 파일명 (`id_rsa_undol26A`)과 같게** 해야 한다.
내가 정한 Host의 이름에서, 저 identityFile과 일치하는 것으로 내가 올바른 권한을 가진지 확인하기 때문이다.

## 사용할 저장소에 적용

내가 사용하는 저장소에서 수정 및 추가를 한다.

```bash
cd $(YOUR_REPOSITORY)
vi .git/config
```

### 변경전
```bash
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = git@github.com:undol26/undol26.github.io.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```
### 변경후
```bash
[core]
  repositoryformatversion = 0
  filemode = true
  bare = false
  logallrefupdates = true
[remote "origin"]
  url = git@github-undol26A:undol26/undol26.github.io.git
  fetch = +refs/heads/*:refs/remotes/origin/*
[user]
  email = undol26A@gmail.com
  name = undol26A
[branch "master"]
  remote = origin
  merge = refs/heads/master
```

### 차이점
1. `[remote "origin"]` 에 url `git@github.com` -> `git@github-undol26A`와 같이 
위의 `config 파일 생성`에서 정한 Host의 이름으로 수정해야 한다.
2. `[user]` 란을 추가. 본인의 계정에 맞게 추가 한다.

## 출처.
- [https://cresumerjang.github.io/2020/11/15/multiple-GitHub-accounts-on-a-single-machine-with-SSH-keys/](https://cresumerjang.github.io/2020/11/15/multiple-GitHub-accounts-on-a-single-machine-with-SSH-keys/)