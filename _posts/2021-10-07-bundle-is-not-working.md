---
layout: post
title: Bundle is not working at WSL
date: 2021-10-07
tags: [bundle, ruby, jekyll]
toc: true
---

## Problem...
문제가 생겼습니다...

아래 명령어가 먹질 않습니다 ㅜㅜ
```shell
bundle exec jekyll serve
Configuration file: /home/hyungi/IdeaProjects/hyungi.github.io/_config.yml
            Source: /home/hyungi/IdeaProjects/hyungi.github.io
       Destination: /home/hyungi/IdeaProjects/hyungi.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.356 seconds.
/home/hyungi/.rbenv/versions/2.7.4/lib/ruby/gems/2.7.0/gems/pathutil-0.16.2/lib/pathutil.rb:502: warning: Using the last argument as keyword parameters is deprecated
                    Auto-regeneration may not work on some Windows versions.
                    Please see: https://github.com/Microsoft/BashOnWindows/issues/216
                    If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch.
 Auto-regeneration: enabled for '/home/hyungi/IdeaProjects/hyungi.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

Auto-generation 이 윈도우에서는 제대로 동작 안할수 있는데... 전 WSL 이긴 하지만 그래도 linux 를 쓰고 있는데...ㅠㅠ

--no-watch 라는 옵션을 한번 줘보도록 하겠습니다..

![--no-watch option is not working](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/server-not-working.png)

네.... 여전히 안되는군요

Bash on Windows 를 업데이트 해보라네요...

![--no-watch option is not working](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/zsh-and-bash-are-already-up-to-date.png)

하 둘다 이미 최신입니다... 어떻게 해결해야 할까요 내일 다시 한번 방법을 찾아보도록 하겠습니다 ㅜㅜ
