---
layout: post
title:  "How to use multi account in git?"
date:   2022-03-28
category: git
---

<p class="intro"> 나는 github 계정이 개인용도와 회사용도 두 개 있다. 내 작업물을 올릴 때마다 git config에 저장된 게 다른 실수를 계속 하게 되어서 이 참에 방법을 찾아 보았다. </p>

undol26A, undol26B 두 개의 계정으로 구분을 지어보자.

# 1. ssh-key 생성

{%- highlight ruby -%}
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C "undol26A@gmail.com"
{%- endhighlight -%}

이 때 **처음 key를 저장할 파일 명이 중요**하다. (`id_rsa_undol26A`)

{%- highlight ruby -%}
# Generating public/private rsa key pair.
# Enter file in which to save the key (/Users/blabla/.ssh/id_rsa): id_rsa_undol26A
{%- endhighlight -%}

undol26B도 같은 식으로 만들면 된다.

# 2. github에 ssh 등록
| <img src="/public/img/git/ssh01.jpg" alt="" width="300"/>  | <img src="/public/img/git/ssh02.jpg" alt="" width="300"/>  |
| <img src="/public/img/git/ssh03.jpg" alt="" width="300"/>  | <img src="/public/img/git/ssh04.jpg" alt="" width="300"> |

<!-- <figure>
	<img src="/public/img/git/ssh01.jpg" alt="" width="200"> 
  <img src="/public/img/git/ssh02.jpg" alt="" width="200"> 
  <img src="/public/img/git/ssh03.jpg" alt="" width="200"> 
  <img src="/public/img/git/ssh04.jpg" alt="" width="200"> 
</figure> -->

순서대로 따라하면 된다. github에 로그인한 자신의 계정에서 차례로 `1. 계정 선택` -> `2 Settings` -> `3. SSH and GPG keys` -> `4. New SSH key` 를 누르면 
`5. Title` 에 자신이 구분할 ssh 이름을 원하는 대로 짓고 (`ex. undol26-personal`) `6. key`에 위 1번에서 생성한 public key 를 붙여 넣는다. public key는 아래 명령어를 통해 알 수 있다.

{%- highlight ruby -%}
cat ~/.ssh/id_rsa_undol26A.pub
{%- endhighlight -%}

github에 undol26A 계정으로 로그인 한 뒤, 위의 ssh-key를 복사하고
다시 github에 undol26B 계정으로 로그인 한 뒤, undol26B에 해당하는 ssh-key를 다시 복사한다.

# 3. config 파일 생성
{%- highlight ruby -%}
vim ~/.ssh/config
{%- endhighlight -%}

아래와 같이 생성한다.
{%- highlight ruby -%}
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
{%- endhighlight -%}

- Host: 내가 정하는 이름이다. (마음대로 정해도 된다.)
- HostName, User: 똑같이
- IdentityFile: 앞에는 본인의 경로를 적어주고(`/home/undol26/.ssh/`) 마지막 `id_rsa_undol26A`을 위의 **1. ssh-key 생성에서 저장한 파일명과 같게** 해야 한다.
내가 정한 Host의 이름에서, 저 identityFile과 일치하는 것으로 내가 올바른 권한을 가진지 확인하기 때문이다.

# 4. 사용할 저장소에 적용

내가 사용하는 저장소에서 수정 및 추가를 한다.

{%- highlight ruby -%}
cd $(YOUR_REPOSITORY)
vi .git/config
{%- endhighlight -%}

| 변경전      | 변경후 |
| ----------- | ----------- |
| {%- highlight ruby -%}
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
{%- endhighlight -%}    
| 
{%- highlight ruby -%}
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
{%- endhighlight -%}       |

1. [remote "origin"] 에 url `git@github.com` -> `git@github-undol26A`
위의 3. config 파일 생성에서 정한 Host의 이름으로 수정해야 한다.
2. [user] 란을 추가. 본인에 맞게 추가 한다.

# 5. 정리
[reference](https://cresumerjang.github.io/2020/11/15/multiple-GitHub-accounts-on-a-single-machine-with-SSH-keys/)를 보고 그대로 따라했는데도 헷갈리는게 많아서 직접 정리해보았다. 

이제 다시 개발 시작하자... 