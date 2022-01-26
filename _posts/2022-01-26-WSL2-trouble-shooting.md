---
layout: post 
title: Well known WSL2 problems 
date: 2022-01-26 22:21 
tags: [WSL2, localhost, VMMEM]
toc: true
---

## Well known problems of WSL2
WSL 에는 널리 알려져 있는 문제가 몇가지 있습니다.
그 중에서 제가 최근에 만난 두가지 문제의 해결법을 정리해 두려고 합니다.

### Vmmem memory issue
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

### Cannot connect to localhost app from Windows browser
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

### Note
최근 회사의 보안 정책이 변화 되면서, WSL2 을 통해서 개발을 하고 있는데, WSL2 가 은근히 괜찮으면서도 아직 제대로 지원이 안되는 부분이 좀 있어서 트러블슈팅을 굉장이 많이 했습니다.
특히나 위에서 언급한 localhost access 의 경우 VPN 으로 붙을 경우 거의 100% 확률로 걸리는 문제여서 꼭 해결해야 했고, 사실 집에서 혼자 개발 할때는 만나본적이 거의 없긴 했습니다 ㅎㅎ..