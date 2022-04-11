---
layout: post
title:  "[git] git submodule"
date:   2021-08-17
category: git
---

## Intro.
git submodule에 대하여

Submodule을 추가하려는 패키지를 <span style="color:#f92672">source</span>패키지라고 하고, 추가하고자 하는 패키지를 <span style="color:#f92672">target</span>패키지라고 하자.

## 추가하기
```bash
git submodule add "child package github address"
# ex) git submodule add https://github.com/undol26/pointnet.git
git commit -m "commit message"
git push -u origin "branch name"
# ex) git push -u origin master
```

## submodule update 하기

```bash
git submodule update --recursive --init --remote
```

<span style="color:#f92672">--remote</span>: 링크 걸린 패키지(<span style="color:#f92672">target</span> 패키지)의 최신을 받고 싶을 때 사용하는 옵션. 만약 링크 건 시점의 commit id 를 git pull 하고 싶다면 --remote 옵션을 빼고 입력하면 된다. 

<span style="color:#f92672">--recursive</span>: <span style="color:#f92672">target</span> 패키지가 submodule이라면, 이녀석까지 한번에 업데이트 하려면 --recursive 옵션을 넣는다.

## 로컬에서 변경사항이 있다면
<span style="color:#f92672">target</span> 패키지의 변경사항이 있다면, 내가 1번에서 <span style="color:#f92672">target</span> 패키지를 추가한 시점의 commit 아이디와 달라지게 된다. 따라서 최신의 패키지로 업데이트를 해줘야 하는데, 간단히 add만 해주면 된다.

```bash
git add "child package github address"
# ex) git add https://github.com/undol26/pointnet.git
git commit -m "commit message"
git push -u origin "branch name"
# ex) git push -u origin master
```

## submodule 제거하기
submodule한 패키지가 더 이상 필요 없다면

```bash
git submodule deinit -f "child package name"
# ex) git submodule deinit -f https://github.com/undol26/pointnet.git
rm -rf "child package name"
# ex) rm -rf https://github.com/undol26/pointnet.git
git rm -f "child package name"
# ex) git rm -f https://github.com/undol26/pointnet.git
```