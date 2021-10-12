---
layout: post 
title: Windows Terminal Setting for WSL 
date: 2021-10-12 
tags: [WSL, Windows Terminal, Ubuntu]
toc: true
---

## Windows Terminal?

말 그대로 윈도우 에서 제공하는 터미널 인데요 기존 명령프롬프트(`cmd` 라고 호출 하던 그녀석..) 와는 달리 이녀석을 통해 WSL 로 설치한 Ubuntu 에 접속할 수 있어 잘 쓰고 있는 녀석입니다.

게다가 지금 Window 환경에서 Jetbrains 사 툴들의 Terminal 이 영 상태가 좋지 못해서..

![cursor position error](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/wsl-zsh-jetbrain-terminal-cursor-error.png)

주력으로 쓰고 있는 CLI 툴 이기도 합니다.

### Look and Feel

![wsl look and feel](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/wsl-look-and-feel.png)

초반에 ZSH 설치를 하면서 동시에 Terminal 도 손을 좀 봐서 깔끔하니 이쁩니다. 커서 문제를 빼고 봐도 Jetbrains 의 Terminal 보다 더 이쁜것 같네요 ㅎㅎ 텍스트 > 색 구성표 에서는 One
Half Dark 를 글꼴 에서는 D2Coding 을 선택한 상태 입니다.

그런데 여기서 문제가 하나 있습니다.

### Default Path

보시다 시피 처음에 Terminal 경로가 이상합니다.

`/mnt/c/Users/hyeon` ??

아마도 제 WSL 이 윈도우 파일 시스템에서 인식 될때 사용 되는 home directory 비슷한거 같은데... 전 이걸 그냥 리눅스 그 자체로 쓰고 있기 때문에 항상 `cd` 커맨드를 이용해서 원래의 홈
디렉토리로 돌아가고 있습니다.

그래서 오늘은 이걸 그냥 리눅스에서 보통 Terminal 을 실행 했을 때 나오는 경로로 바꿔 보려고 합니다.

Windows Terminal 에서 드롭다운 메뉴를 열어 설정 으로 들어간 다음, 우측에 귀여운 우분투 아이콘을 선택해 주면 json file 을 어떤 editor 로 열것인지 물어봅니다.

![how to get setting](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/drop-down-menu-windows-terminal.png)

저는 Sublime Text 를 선택 했구요. 아래와 같은 파일을 확인 할 수 있습니다.

![setting file](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/wsl-config-json.png)

그 중에서 해당하는 WSL 옵션을 찾아서 아래 맨 마지막 줄 처럼 추가해 주시면 됩니다.
USER_NAME 은 WSL 설치하실때 입력한 user name 사용하시면 됩니다.

```json
{
  "list": {
    "colorScheme": "One Half Dark",
    "font": {
      "face": "D2Coding"
    },
    "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
    "hidden": false,
    "name": "Ubuntu",
    "source": "Windows.Terminal.Wsl",
    "startingDirectory": "//wsl$/Ubuntu/home/{$USER_NAME}"
  }
}
```

그럼 아래와 같이 처음 화면부터 깔끔하게 실제 WSL 의 home directory 로 시작하는 것을 확인하실 수 있습니다.

![Changed Home directory](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/wsl-changed-home-dir.png)

[참고한 포스트](https://jakupsil.tistory.com/45)