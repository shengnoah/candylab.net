---
layout: page
title: "powerline"
subheadline: "powerline"
meta_title: "powerline"
subheadline: "powerline"
teaser: ""
permalink: "/powerline/"
---

```
   pip install powerline
```  


```shell
export TERM=xterm-256color


function _update_ps1() {
    PS1=$(powerline-shell $?)
}

if [[ $TERM != linux && ! $PROMPT_COMMAND =~ _update_ps1 ]]; then
    PROMPT_COMMAND="_update_ps1; $PROMPT_COMMAND"
fi

```

![fonts](https://github.com/powerline/fonts)

需要字体支持,有一个字体叫source code pro for powerline 。



```
bash的Powerline启动
```


```
if [ -f "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh" ]; then
  __GIT_PROMPT_DIR=$(brew --prefix)/opt/bash-git-prompt/share
  GIT_PROMPT_ONLY_IN_REPO=1
  source "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh"
fi
```
