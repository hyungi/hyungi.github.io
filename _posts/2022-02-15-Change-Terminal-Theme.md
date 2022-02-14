---
layout: post
title: Change Terminal Theme
data: 2022-02-15 00:31
tags: [tag1,tag2,tag3]
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
            "data: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE",
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
