---
layout: post
title: "javascript正则表达式（上）"
description: "javascript正则表达式基础部分的理解"
tags: [RegularExpression]
comments: true
share: true
---
正则表达式在过去十多年间越来越普及，如今所有常用的编程语言都会包含一个强大的正则表达式函数库，或者甚至是在语言本身就内嵌了对正则表达式的支持。与IT业界流行的东西一样，正则表达式也有许多种不同的实现，以及不同程度的兼容性，这就出现了许多不同的正则表达式流派。常见的有`.NET`、`Java`、`Javascript`、`Perl`、`Python`和`Ruby`等。本文章描述的是本人对于javascript正则表达式与其他流派的一些不同。

### One
javascript中的`.`是不匹配换行的，作为替代可以选择使用`[\s\S]`，至于标志换行`\m`则是影响`^ $`，举一个例子
```ruby
Pattern:^cat$
Subject:
cat
cat
Result:NULL

Pattern:^cat$\m
Subject:
cat
cat
Result:cat
```

### Two
Javascript中只把ASCII字符看作是单词字符，因此`\w`与`[a-zA-Z0-9]`是完全等同的，这意味着对于Unicode字符需要其他的方式进行匹配。

### Three
Javascript是不支持命名捕获分组和命名反向引用。什么是命名捕获？像这个样子`(?<day>\d\d)`为捕获的文本添加描述性的名词day。

### Four
为了消除不必要的回溯，大部分语言都占有量词和原子分组的解决方案，但Javascript并不支持这两个。占有量词形式类似`\b\d++\b`，原子分组的形式`(?>`,这两种解决方案都不会记住回溯位置，但是处理的方式还是存在着区别，有兴趣的朋友可以去相关资料仔细了解下。

### Five
如果你想要检查一个匹配，但是不添加到整体匹配中，那你需要使用`环视`。环视拥有特殊的性质，可以放弃在环视内部的正则表达式部分匹配的文本，实质上，环视会检查某些文本是否可以被匹配，但是不会实际去匹配它。
```ruby
Pattern:(?<=<b>)\w+(?=</b>)
Subject:
<b>cat</b>
Result:cat
```
从上诉例子中可以看到只匹配了cat，<b>和</b>并未匹配。遗憾的是Javascript并不支持`(?<=`,也就算肯定型逆序环视，不过肯定型顺序环视`(?=`倒是支持的，另外否定性顺序环视同样支持。如果你想要用Javascript模拟逆序环视，也是可以做到的。

### Six
Javascript不支持向正则表达式中添加注视。

### Seven
在替代文本中，Javascript把反斜杠`\`当作一个字面字符，不需要再用另外一个`\`来对它进行转义，否则会得到两个反斜杠。单个出现的美元符号也是一个字面字符，只有当它们之后是一个数字、&、反引号、垂直引号、下划线、加号或者另外一个美元符号的时候，才需要被转义，转义一个美元符号，只需要再加一个美元符号。

### eight
在Javascript的替代文本中`$&`来表示整个匹配，如果存在分组，则`$1`来表示第一个捕获分组匹配到的文本，依次类推。你也许会想到`$10`这中情况，这里存在着二义性，可以被解释为第10个分组或者是第一个分组后面跟着一个0。这一方面Javascript使用了聪明的办法，如果存在着第10个捕获分组，则这两个数字被用于捕获分组，否则为另一种解释。如果你引用了不存在的分组，Javascript会把不存在的分组的引用当作是替代文本的字面文本。

### Nine
Javascript在替代文本中使用"$`"和"$'"来表示左右上下文。

### Ten
Javascript的反向引用与其他流派不同。Javascript对一个还没有参与匹配的分组的反向引用总是会匹配成功，相当于捕获了长度为0的匹配的分组的反向引用。比如:
```ruby
\d\d\1-\1-\d\d
```
在Javascript中是不算错误的,而且会匹配成功`20--20`

最后推荐几个可以在线验证正则表达式的网站[http://regex.larsolavtorvik.com/]和
[http://www.regexpal.com/]

[http://www.regexpal.com/]:http://www.regexpal.com/
[http://regex.larsolavtorvik.com/]:http://regex.larsolavtorvik.com/
