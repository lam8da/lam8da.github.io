---
layout: post
title: Bash下加速命令输入的两个技巧
categories:
    - 环境&配置
tags:
    - Bash
    - Linux
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: sh \}" /}

打开/etc/inputrc，在最后添加如下两行命令：

```sh
set completion-ignore-case on #启用后在按tab键时返回的列表忽略大小写。这样我们在输入一个包含大写字母的命令或名称时就不用老是按shift了。直接用tab补全就好。
set show-all-if-ambiguous on #启用后按一次tab即返回列表，而不是原来的按两次。
```

设置保存后新开一个terminal窗口就会生效。
