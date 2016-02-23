---
title: "正则表达式必知必会"
date: 2016-02-22 11:00
---

[TOC]


## 第1章 入门 ##

> 正则表达式的两种基本用途:「搜索」和「替换」。

> 在使用正则表达式的时候, 你将发现几乎所有的问题都有不止一 种解决方案。它们有的比较简单, 有的比较快速, 有的兼容性更好,有的功能更全。这么说吧, 在编写正则表达式的时候, 只有「对」、「错」两种选择的情况是相当少见的 —— 同一个问题往往会有多种解决方案。


## 第2章 匹配单个字符 ##

最简单的是匹配纯文本。

全局搜索: 针对有多个匹配结果, 大多数正则表达式引擎的默认行为是只返回第1个匹配结果。不过很多实现都提供了能够把所有的匹配结果全部找出来的机制。常见的元字符是 `g` (表示global)

大小写问题: 正则表达式是区分字母大小写的, 大多数正则表达的式实现也支持不区分字母大小写的匹配操作。常见的远字符是 `i` (表示ignore)

匹配任意单个字符: `.` 。

转义特殊字符: `\` 。比如上面的 `.` , 如果要把这个当成普通字符匹配, 则需要写为 `\.` ; 同理, 因为 `\` 是做转义用的特殊字符, 要匹配其本身, 则需要写为 `\\` 。后续的一些元字符, 如果要匹配其自身, 都需要转义。

正则表达式经常被简称为模式, 它们其实是一些由字符构成的字符串。这些字符可以是 普通字符(纯文本) 或 元字符(有特殊含义的特殊字符)。


## 第3章 匹配一组字符 ##

匹配多个字符中的某一个: `[]`。里面定义多个字符的集合, 表示匹配该集合中任意一个字符。

如:

	[rR]e[gG]ex  # 匹配 regex, Regex, reGex, ReGex

在集合里面, 还可以定义区间, 比如表示某个数字, 虽然可以使用`[0123456789]`但是不够简洁清晰, 可以:

	[0-9]

甚至可以表示多个区间, 如表示所有大小写字母和数字:

	[A-Za-z0-9]

注意 连字符`-` 只有在集合里面才表示一个特殊的元字符, 在集合外面只是一个普通字符。

在集合里, 还可以对集合做 取非操作, 使用`^`, 如表示除数字以外其它字符:

	[^0-9]

注意 ^ 的效果将作用于给定字符集合里的所有字符或字符区间, 而不是仅限于紧跟在 ^ 字符后面的那一个字符或字符区间。


## 第4章 使用元字符 ##

元字符大致可以分为两种: 一种是用来匹配文本的(比如 `.`), 另一种是正则表达式的语法所要求的(比如 `[` 和 `]`)。

匹配空白字符:

* `\n` 换行符
* `\r` 回车符
* `\t` 制表符(Tab键)
* `\f` 换页符
* `\v` 垂直制表符
* `[\b]` 回退(并删除)一个字符(Backspace键)

\r\n匹配一个“回车+换行”组合,有许多操作系统(比如Windows) 都把这个组合用作文本行的结束标签。

\r\n是Windows所使用的文本行结束标签。Unix和Linux 系统只使用一个换行符来结束一个文本行;换句话说,在 Unix/Linux系统上匹配空白行只使用\n\n即可,不需要加上\r。 同时适用于Windows和Unix/Linux系统的正则表达式应该包含 一个可选的\r和一个必须被匹配的\n。

字符集合(匹配多个字符中的某一个)是最常见的匹配形式, 而一些常用的字符集合可以用特殊元字符来代替:

* `\d` 任何一个数字字符 (等价于 `[0-9]`)
* `\D` 任何一个非数字字符 (等价于 `[^0-9]`)
* `\w` 任何一个字母数字字符(大小写均可)或下划线字符 (等价于 `[a-zA-Z0-9_]`)
* `\W` 任何一个非字母数字或非下划线字符 (等价于 `[^a-zA-Z0-9_]`)
* `\s` 任何一个空白字符 (等价于 `[\f\n\r\t\v]`)
* `\S` 任何一个非空白字符(等价于 `[^\f\n\r\t\v]`)

注意 用来匹配退格字符的 `[\b]` 元字符是一个特例: 它不在类元字符 `\s` 的覆盖范围内。

关于`-w`, 是一种比较常用的字符集合, 这些字符常见于各种名字里,如文件名、子目录名、变量名、数据库对象名等。

进制匹配:

* `\x` 匹配十六进制。如 `\x0A` 等价于 `\n`
* `\0` 匹配八进制。如 `\011` 等价于 `\t`

POSIX字符类是许多正则表达式实现都支持的一种简写形式:

字符类     | 说 明
-----------|-------------------------------------------
[:alnum:]  | 任何一个字母或数字 (等价于[a-zA-Z0-9])
[:alpha:]  | 任何一个字母 (等价于[a-zA-Z])
[:blank:]  | 空格或制表符 (等价于[\t])
[:cntrl:]  | ASCII控制字符 (ASCII 0到31，再加上ASCII 127)
[:digit:]  | 任何一个数字 (等价于[0-9])
[:graph:]  | 和[:print:]一样，但不包括空格
[:lower:]  | 任何一个小写字母 (等价于[a-z])
[:print:]  | 任何一个可打印字符
[:punct:]  | 既不属于[:alnum:]也不属于[:cntrl:]的任何一个字符
[:space:]  | 任何一个空白字符，包括空格 (等价于[^\f\n\r\t\v])
[:upper:]  | 任何一个大写字母 (等价于[A-Z])
[:xdigit:] | 任何一个十六进制数字 (等价于[a-fA-F0-9])

例如下面匹配html中的rgb color:

	#[[:xdigit:]]{6}

注意 这里使用的模式以 `[[` 开头、以 `]]` 结束 (两对方括号)。 这是使用POSIX字符类所必须的。POSIX字符类必须括在 `[:` 和 `:]` 之间,我们使用的POSIX字符类是 `[:xdigit:]`(不是`:xdigit:`)。外层的 `[` 和 `]` 字符用来定义一个字符集合, 内层的 `[` 和 `]` 字符是POSIX字符类本身的组成部分。


## 第5章 重复匹配 ##

* `+` 匹配一个或多个
* `*` 匹配零个或多个
* `?` 匹配零个或一个
* `{N}` 指定具体的重复匹配次数, 如 {3} 表示匹配3次
* `{M, N}` 指定重复匹配次数的区间, 如 {3, 5} 表示最少重复3次, 最多重复5次
* `{M, }` 指定最少重复多少次, 如 {3, } 表示重复3次或更多

如:

	a+  # 匹配一个或多个连续的a
	[0-9]+  # 匹配一个或多个连续的数字

注意 `+`等元字, 如果在集合中, 则表示普通字符。

另外，`?`, `+`, `*`, `{m, n}` 都是「贪婪匹配 (greedy match)」的元字符，也就是他们会尽可能多的去匹配。如:

	$ cat txt
	---text block 1---text block 2---end

	$ grep -P -o -- '---.*---' txt
	---text block 1---text block 2---

某些情况需要尽可能的少的匹配, 即匹配第一次符合的就停止继续匹配, 也就是「懒惰匹配 (non-greedy match 或 lazy match)」, 只需要在贪婪的元字符后面加上 `?` 后缀:

	$ grep -P -o -- '---.*?---' txt
	---text block 1---

注解:

这里匹配三个减号符之间的内容, 因为 `-` 在命令行中有特殊含义, 表示命令行参数, 所以这里在所有参数选项后加上 `--` (bare double dash), [参考](http://stackoverflow.com/questions/2427913/how-can-i-grep-for-a-string-that-begins-with-a-dash-hyphen); 之前报错还没想到是这里的问题。

另外, `grep -G`, `grep -E` 都不支持懒惰匹配, 所以使用了Perl型的解析: `grep -P`。

最后, `grep -o` 只打印出匹配的内容, 这么看就比较直观了。

再比如:

	$ cat txt
	---abcdefg---1234567---xxxxxxx

	$ grep -P -o -- '---.{3,5}' txt
	---abcde
	---12345
	---xxxxx

	$ grep -P -o -- '---.{3,5}?' txt
	---abc
	---123
	---xxx

在 [正则匹配YAML Front Matter](http://code.tankywoo.com/UVMgMQ77/) 里也提到了懒惰匹配。