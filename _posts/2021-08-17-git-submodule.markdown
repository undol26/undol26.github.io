---
layout: post
title:  "[git] git submodule"
date:   2021-08-17
category: git
---

## Intro.
git submodule은 말 그래도 sub의 module들을 git을 이용해서 한 데 모아둔 것을 말한다. 

여러 하위 패키지를 모아서 하나의 상위 패키지로 모아두면, 어떤 dependencies가 있는 패키지를 개발 할 때 일일이 찾지 않고 한 번에 설치할 수 있는 이점이 생길 수 있다.

## submodule add
```bash
git submodule add ${child_package_github_address}
# ex) git submodule add https://github.com/undol26/pointnet.git
git commit -m "commit message"
git push -u origin "branch name"
# ex) git push -u origin master
```

#### Add new submodule package
내가 추가하려고 하는 `child` 패키지의 저장소 주소를 `add`하면 된다.

#### Update changes
만약 추가한 패키지의 내용이 변경되었으면?

submodule add 의 방식과 똑같이, 그 패키지 자체를 다시 add하면 된다.

## submodule update
```bash
git submodule update --recursive --init --remote
```

`--remote`: submodule 패키지의 최신을 받고 싶을 때 사용하는 옵션. 만약 링크 건 시점의 `commit id` 를 `git pull` 하고 싶다면 --remote 옵션을 빼면 된다. 

`--recursive`: submodule 안에 또 submodule이 있을 수 있다. 이 옵션을 사용하면 모든 submodule안의 내용을 업데이트 한다.

## submodule remove
submodule한 패키지가 더 이상 필요 없다면

```bash
git submodule deinit -f "child package name"
# ex) git submodule deinit -f https://github.com/undol26/pointnet.git
rm -rf "child package name"
# ex) rm -rf https://github.com/undol26/pointnet.git
git rm -f "child package name"
# ex) git rm -f https://github.com/undol26/pointnet.git
```

## 주의사항
제일 하기 쉬운 실수 중에 하나가 
```bash
git clone ${submodule_package_address}
```
를 하면 끝나는 줄 아는데 절대 아니다.

submodule로 엮으면 안의 자세한 내용은 하나도 없이 껍데기(ex. `directory`)만 연결되어 있는 상황이다. 

그렇기 때문에 위에서 언급한 `submodule update`를 반드시 해줘야 한다!!