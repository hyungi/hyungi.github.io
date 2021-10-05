---
layout: post
title: Install WSL and ZSH
date: 2021-10-05
tags: [WSL,WSL2,ZSH]
toc: true
---

## WSL?
Window Subsystem Linux 의 약자로 말 그대로 윈도우에서 linux 를 별다른 VM 프로그램을 사용하지 않아도 윈도우 자체적으로 지원해주는 시스템을 의미합니다.

WSL2 로 넘어오면서 제약사항이 많이 적어 졌다고 해서 한번 시도해 보고 있습니다.
저는 지금 현업에서 쓰고 있는 우분투를 한번 설치 해보도록 하겠습니다.

### Before install WSL
- BIOS 확인: 하드웨어 가상화를 이용해야 하기 때문에 아래 링크를 참조해서 자신의 PC 가 하드웨어 가상화를 enable 했는지 확인 할것
  https://github.com/microsoft/WSL/issues/4120
```
Can you ensure both these are enabled?
Hardware Virtualization Assists* in the form of:
Intel VT-x
AMD AMD-V
Hyper-V requires Hardware Data Execution Prevention:
Intel refers to it as Execute Disable (XD). This feature must be enabled in the system BIOS.
AMD refers to it as No Execute (NX). This feature must be enabled in the system BIOS.
```

### Enable WSL
- MS Store 에서 Window Terminal 설치 하고, 관리자 권한으로 실행 하여 아래 명령어 입력
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Install Ubuntu as WSL
- 다시 MS Store 로 가서 우분투 배포판 설치 및 초기 설정을 마친뒤 Window Terminal 에서 아래 명령어로 WSL 이 정상적으로 설치 되었는지 확인
```shell
> wsl -l
  NAME                   STATE           VERSION
* Ubuntu                 Running         2
```
- 만약 Version 이 1로 나온다면, 아래와 같이 설정
 ```shell
> wsl --set-version Ubuntu 2
```

## ZSH?
shell 환경을 위한 도구로, bash 보다는 테마 추가가 가능해 좀더 보기 좋게 사용이 가능하고 각종 플러그인도 많아서 사용하기 편리 합니다.

### Install ZSH
사실 Ubuntu 배포판에서는 깔려 있기는 합니다. 그래도 최신 버전을 사용하는 것이 좋으니 최신 버전을 다시 깔아보도록 하겠습니다.
```shell
$ sudo apt-get update
$ sudo apt-get install zsh
```

설치가 끝난 다음에는 기본 Shell 을 zsh 로 변경해 줍니다.
```shell
chsh -s $(which zsh)
```

그리고 각종 테마와 플러그인을 사용하기 위해 빠질 수 없는 oh-my-zsh 를 설치해 줍니다.
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

테마와 각종 플러그인 들은 .zshrc 파일 안에서 설정이 가능합니다.

다만 여기서 한가지 문제가 있다면.... jetbrain 사에서 제공하는 각종 IDE 의 terminal 에서 커서 위치가 이상하다는 것이죠 ㅜㅜ

![cursor position error](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/wsl-zsh-jetbrain-terminal-cursor-error.png)

아래 링크들 처럼 여기 저기 리포트는 올라가고 있는데 아직 해결 된건 없는것 같습니다...

[https://youtrack.jetbrains.com/issue/IDEA-267458, https://github.com/agnoster/agnoster-zsh-theme/issues/156]

그래서 영 거슬리신다면, 아래 링크 처럼 아예 개행을 하도록 설정하시면 해결이 됩니다..
https://wayhome25.github.io/etc/2017/03/12/zsh-alias/

## 출처
[https://www.44bits.io/ko/post/wsl2-install-and-basic-usage, https://tutorialpost.apptilus.com/code/posts/tools/using-zsh-oh-my-zsh/]