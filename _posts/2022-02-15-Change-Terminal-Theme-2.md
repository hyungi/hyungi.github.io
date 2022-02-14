---
layout: post 
title: Change Terminal Theme
date: 2022-02-15 00:31
tags: [starship, vscode, terminal]
toc: true
---

## Change Terminal Theme
zsh 로만 계속 쓰다 보니 조금 질리는 감이 있어서 starship 으로 terminal theme 을 한번 바꿔봤습니다.

![starship-image](https://starship.rs/logo.svg)

Installation guide 에 따르면 nerd font 를 우선 설치해주고
[Nerd Fonts](https://www.nerdfonts.com/)

아래 명령어를 입력해 주면 설치가 됩니다.(리눅스 기준)
```shell
sh -c "$(curl -fsSL https://starship.rs/install.sh)"
```

그리고 configuration guide 에 따라서 한번 간단한 세팅을 해줍니다.
저는 아래와 같이 git 관련 세팅을 해줬습니다. https://starship.rs/config/#git-branch

![starship-toml-config](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/starship-toml-config.png)


짜잔~

![starship-apply-result](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/starship-apply-result.png)

그렇지만... jetbrain 사의 IDE 에서는 제대로 보이질 않습니다 ㅜㅜ

![starship-theme-not-working-intellij](https://raw.githubusercontent.com/hyungi/hyungi.github.io/main/assets/images/starship-theme-not-working-intellij.png)

그래서 IDE 도 좀 질렸던 차에 vscode 로 이사를 하고 있습니다..! 이게 무료 이기도 하구요

그래서 Intellij 에서 template 만든것 처럼 여기서도 동일하게 해보려고 했는데 파일 생성시 부터 적용 되는 template 은 없는것 같고 파일 작성 이후 적용 가능한 snippet 이라는게 있어서 이걸 세팅 해봤습니다.

- Snippet sample
```json
{
    "github_blog": {
        "prefix": "page_layout",
        "body": [
            "---",
            "layout: post",
            "title: ${TM_FILENAME_BASE/[^a-z^A-Z]//g}",
            "date: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE",
            "tags: [${1:tag1},${2:tag2},${3:tag3}]",
            "toc: true",
            "---"
        ],
        "description": "post layout"
	},
	"shell":{
		"prefix": "shell",
		"body": [
			"```shell",
			"$0",
			"```"
		],
		"description": "shell script insert"
	}
}
```
문법은 아래를 참고하시면 됩니다..!
[user define snippet offical document](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
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
