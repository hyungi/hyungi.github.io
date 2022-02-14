---
layout: post 
title: Well known WSL2 problems 
date: 2022-01-26 22:21 
tags: [localhost, WSL2, VMMEM, ubuntu, host-binding]
toc: true
---

## Well known problems of WSL2
WSL 에는 널리 알려져 있는 문제가 몇가지 있습니다.
그 중에서 제가 최근에 만난 세가지 문제의 해결법을 정리해 두려고 합니다.
세번째 문제는 이 포스팅을 하면서 만나게 되었습니다 😂

### 1. Vmmem memory issue
도커를 자주 사용하다보면 Vmmem 이라는 프로세스의 점유율이 치솟는 경우가 종종 있는데요,
이 문제에 대해서 MS 도 인지는 하고 있지만, 일단은 Workaround 만 소개되어 있는 상황입니다.
대략적인 문제의 원인은 WSL2 가 파일시스템을 이용할때 메모리를 할당해서 쓰기만 하고 반납을 안하는 상황이라고만 알고 있습니다.

C:\Users\{UserName} 이 경로로 가서 (%UserProfile% 과 같습니다.)

`.wslconfig` 라는 파일을 아래와 같이 생성합니다.
```
[wsl2]
memory=6GB
swap=0
```
이렇게 되면, WSL 의 메모리는 6으로 제한 되며 스왑 또한 하지 않습니다.

https://github.com/microsoft/WSL/issues/4166#issuecomment-526725261

### 2. Cannot connect to localhost app from Windows browser
WSL 에서 실행한 웹앱에 접근을 하려고 해도 404 에러 등이 뜨며 접근이 잘 안되는 경우가 있습니다.
이것은 여러가지 해결 방법이 있는데

#### 첫번째 해결 방법 .wslconfig 파일 수정
위에서 작성 .wslconfig 파일에 아래와 같이 `localhostForwarding=true` 를 추가한다
```
[wsl2]
memory=6GB
swap=0
localhostForwarding=true
```

#### 두번째 해결 방법 binding host 수정
사실은 외부에서 그러니까 Windows 호스트 외부에서 접근 하는 경우에 대한 해결책이긴 한데, 
로컬호스트로 접근이 잘 안될때도 적용하면 잘 동작합니다..

웹앱을 띄울때 binding host 를 ~~127.0.0.1~~ 이나 ~~localhost~~ 로 하는것이 아니라
**0.0.0.0** 으로 하면 문제가 보통 해결 됩니다.

https://docs.microsoft.com/en-us/windows/wsl/networking#connecting-via-remote-ip-addresses

#### 세번째 해결 방법 WSL port forwarding
사실 port forwarding 이 그렇게 어렵지 않은데, 문제는 WSL2 은 그 특성상 VM 이 올라올 때 마다 IP 주소가 바뀌는 구조 이기 때문에
이걸 매번 찾아서 열어주기가 매우 귀찮다는게 문제였는데....
이 문제를 해결하는 스크립트를 짜주신 분이 있으니 아래 링크를 참조하면 됩니다.

https://github.com/microsoft/WSL/issues/4150#issuecomment-504209723

위 내용중 중간에 위치한 아래 부분의 ports 에서 열어주고자 하는 포트를 추가하면 됩니다.
```shell
...
#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,5000);
...
```

### 3. File format issue.
각종 파일을 Windows IDE 상에서 생성하고 올릴때 가끔 문제가 있는 경우가 있습니다.
저 같은 경우에는 Shell script 상에서 제가 넣은적이 없는 ^M 이 추가 되어있는 경우가 종종 있었습니다.
아래 포스트를 참고하면 직접적으로 보이는 ^M 문자를 vim editor 의 바이너리 모드로 진입해서 삭제하는 방법을 알려줍니다.
https://thenewth.wordpress.com/2020/12/16/bin-shm-bad-interpreter-%ed%95%b4%ea%b2%b0-%eb%b0%a9%eb%b2%95/
혹은 vim 으로 진입한뒤 아래 커맨드를 입력해도 됩니다.

`:set ff=unix`

하지만, 개행 문제가 아니라 그냥 파일 포맷 그 자체가 문제를 일으키는 경우도 있습니다.... 이 포스트를 올린 직후 경험했습니다.
아래 처럼 갑작스럽게 meta description 에 파일 내용 전체가 들어가는 황당한 오류가 발생했는데..
![meta-description-is-too-long](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/meta-description-issue.png)

이 역시 file format 문제로 밝혀졌습니다. 놀랍게도 IDE 에서 이전 포스트 복붙하면 문제가 발생하지 않습니다...(그럼 템플릿 기껏 만든 이유가 없는데...! ㅜ)
```shell
$ file 2022-01-26-WSL2-trouble-shooting.md
2022-01-26-WSL2-trouble-shooting.md: ASCII text, with very long lines, with CRLF line terminators
```
암튼 이것도 file format 을 unix 로 바꿔주면 해결이 됩니다.

그리고 추후 문제 재발을 막기 위해서 아래와 같이 개행 문자 설정도 바꿔 줍니다.
```shell
git config --global core.autocrlf false
```
[참고한포스트](https://dabo-dev.tistory.com/13)

### Note
최근 회사의 보안 정책이 변화 되면서, WSL2 을 통해서 개발을 하고 있는데, WSL2 가 은근히 괜찮으면서도 아직 제대로 지원이 안되는 부분이 좀 있어서 트러블슈팅을 굉장이 많이 했습니다.
특히나 위에서 언급한 localhost access 의 경우 VPN 으로 붙을 경우 거의 100% 확률로 걸리는 문제여서 꼭 해결해야 했고, 사실 집에서 혼자 개발 할때는 만나본적이 거의 없긴 했습니다 ㅎㅎ..
