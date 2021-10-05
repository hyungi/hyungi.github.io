---
layout: post
title: Install ruby jekyll
date: 2021-10-06
tags: [github-pages, ruby, jekyll]
toc: true
---

## Ruby
아주 예전에 멋쟁이 사자처럼 2기(?) 시절에 친구가 듣는 걸 몰래 빌려 들을때 Ruby on Rails 라는 프레임 워크를 써본적이 있는데,
그 뒤로는.... 써본 기억이 없지만 당근마켓에서 쓴다는 걸 어디서 본 것 같네요.

지금 Ruby 를 쓰는 이유는 jekyll 플러그인이 ruby 기반이라 github 블로그를 로컬에서 구동 해서 markdown 의 편집 상태를 확인 해보기위함 입니다.

### Install Ruby
[Ruby 설치 공식 가이드 - 우분투](https://www.ruby-lang.org/ko/documentation/installation/#apt)
```shell
$ sudo apt-get install ruby-full
```

위 처럼 설치를 해도 gem install 이 아래 처럼 안될 가능성이 높습니다.
```shell
$ gem install jekyll bundler
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /var/lib/gems/2.7.0 directory.
```

이유인 즉슨, 시스템상에 깔려 있는 ruby  에 대해서는 sudo 권한이 필요하기 때문입니다.
다만, 이렇게 설치 하는 것은 보안상의 이유로 권장 되지 않는 다고 하니 아래 처럼 rbenv 를 설치해서 가상환경을 추가 설치 하도록 합니다.

```shell
$ sudo apt-get install rbenv ruby-build
$ rbenv install --list
# 설치 가능한 버전을 확인 한뒤 설치
$ rbenv install 2.7.4
# 설치에 꽤 오랜 시간이 걸려서 이게 동작 하는건지 모르겠다 하시는 분들은 --verbose 옵션을 추가하시면 설치 과정을 확인할 수 있습니다.
# 설치가 끝난 다음에 아래 명령어를 입력해야 최종 적용이 됩니다.
$ rbenv rehash
```

다시 위에서 못한 gem install 명령어를 다시 실행하면 정상적으로 동작할 겁니다

```shell
$ gem install jekyll bundler
...
27 gems installed
```
이렇게 정상적으로 install 이 종료되면,
다시 bundle install 을 진행하고, 최종적으로 실행을 해줍니다.
```shell
$ bundle install
$ bundle exec jekyll serve
```

### Running jekyll serve

정상적으로 실행 되면 아래와 같은 메세지를 확인할 수 있습니다.
```shell
$ bundle exec jekyll serve
Configuration file: /home/hyungi/IdeaProjects/hyungi.github.io/_config.yml
            Source: /home/hyungi/IdeaProjects/hyungi.github.io
       Destination: /home/hyungi/IdeaProjects/hyungi.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.393 seconds.
/home/hyungi/.rbenv/versions/2.7.4/lib/ruby/gems/2.7.0/gems/pathutil-0.16.2/lib/pathutil.rb:502: warning: Using the last argument as keyword parameters is deprecated
                    Auto-regeneration may not work on some Windows versions.
                    Please see: https://github.com/Microsoft/BashOnWindows/issues/216
                    If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch.
 Auto-regeneration: enabled for '/home/hyungi/IdeaProjects/hyungi.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```
