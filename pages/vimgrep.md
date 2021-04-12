---
layout: page
title: "VIM Grep"
subheadline: "VIM Grep"
meta_title: "VIM Grep"
subheadline: "VIM Grep"
teaser: ""
permalink: "/vimgrep/"
---



```shell
    vimgrep /匹配模式/[g][j] 要搜索的文件/范围 
    g：表示是否把每一行的多个匹配结果都加入
    j：表示是否搜索完后定位到第一个匹配位置
    vimgrep /pattern/ %             在当前打开文件中查找
    vimgrep /pattern/ *             在当前目录下查找所有
    vimgrep /pattern/ **            在当前目录及子目录下查找所有
    vimgrep /pattern/ *.c          查找当前目录下所有.c文件
    vimgrep /pattern/ **/*         只查找子目录
    cn                              查找下一个
    cp                              查找上一个
    copen                           打开quickfix
    cw                              打开quickfix
    cclose                          关闭qucikfix
    help vimgrep                    查看vimgrep帮助
```   

