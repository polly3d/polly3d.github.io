---
layout: post
title: 正则表达式学习
---

## 什么是正则表达式

正则表达式，是一种描述字符串规则的表达式。比如，要在文本中找到一个字母a开头，b结尾的单词。这种自然语言只有人能够看懂，计算机是看不懂的。所以就了计算机能够看懂的东西，叫正则表达式。

在开始之前，先做一个定义：

* `/`————正斜线（斜线）
* `\`————反斜线

## 常用的元字符

正规表达式是一种对规则描述的表达式，因此需要一些符号来指代常用的字符，比如`\w`指代所有的字母（汉字）、数字和下划线。在正则表达式的术语中，这些字符就叫“元字符”。
常用的元字符如下：

|代码|说明|
|----|----|
|.|匹配除换行符以后的任意字符|
|\w|匹配字母（汉字）、数字和下划线|
|\s|匹配任意空白字符|
|\d|匹配任意数字|
|\b|匹配字符的开头或结尾|
|^|匹配字符开头|
|&|匹配字符结尾|

## 重复

以上介绍的元字符只能代表*一个*字符位置。如果需要对某个字符或字符规则进行重复，需要使用限定符。
常用的限定符如下：

|代码|说明|
|----|----|
|*|重复0次或更多次|
|+|重复1次或更多次|
|?|重复0次或1次|
|{n}|重复n次|
|{n,}|重复n次以上|
|{n,m}|重复n到m次|


## 字符类

到目前为止，如果需要查找数字或字母都比较简单。如果想要找某个没有定义的元字符集呢？比如找1-6的数字，又或者找aeiou这几个字母中的其中一个。这个时候，就需要用到字符类了。

正则表达式的字符类可以写成这样：`[aeiou]`。方括号里面的内容，就是构成了一个自定义的字符集。

## 分支条件

有时候，一条表达式不能够将所有的规则涵盖。所以就需要有通过“分支条件”来实现组合同一类的表达式。可以使用`|`分隔分支条件。格式如下：

`表达式1|表达式2|...`

这样的分支条件，与程序里面的条件判断是相同的。就拿上面的表达式来说，如果表达式1成立，就不会再执行表达式2了。所以在写分支条件时，应该要考虑将最可能的条件放到前面。

## 分组

重复单个字符，可以使用上面介绍的限定符。如果需要重复多个字符呢？就可以使用“分组”。分组也叫子表达式，通过小括号就内容指定为一个分组。如下：

`(\d{1,3}.){3}\d{1,3}` ip地址的简单正则表达式

## 反义

规则的取反操作。基本上将元字符转换成大写即可。常用反义如下：

|代码|说明|
|----|----|
|\W|匹配任意不是字母、数字和下划线的字符|
|\S|匹配非空白字符|
|\D|匹配非数字|
|\B|匹配非开头或结尾|
|^x|匹配除x之外的字符|
|^aeiou|匹配除aeiou之外的字符|

例子：

`\S+` 匹配不包含空白的字符
`<a[^>]>` 匹配尖括号包含的内容

## 后向引用

使用小括号指定一个表达式后，匹配这个子表达式的文本可以在表达式或其它程序中作进一步的处理。默认情况下，每个分组会自动拥有一个组号，规则是从左至右，组号从1开始。示例如下：

`\b(\w+)\b\s+\1\b` 匹配重复的单词，如go go。

可以自己指定组名。如：

`(?<Word>\w+)` 或 `(?'Word'\w+)`

`\b(?<Word>\w+)\b\s+\k<Word>\b` 匹配重复单词

## 后续再完善内容

留个坑，后续再补更深入的内容：

- [ ] 零宽断言
- [ ] 负向零宽断言
- [ ] 注释
- [ ] 贪婪与懒惰
- [ ] 处理选项
- [ ] 平衡组/递归匹配