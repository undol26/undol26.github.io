---
layout: post
title:  "[ubuntu] command not found: ifconfig"
date:   2020-09-28
category: ubuntu
---

## Intro.
최근에 우분투 18.04로 update했고, 벨로다인 센서를 사용하기 위해 ip를 확인을 해야 했다.

당연히 있을줄 알았던
```bash
ifconfig
```

를 입력했더니

{%- highlight ruby -%}
➜  ~ ifconfig
zsh: command not found: ifconfig
{%- endhighlight -%}

띠용.. 이게 기본이 아니라니...

## 해결
```bash
sudo apt-get install net-tools
```

를 설치하면 해결된다!