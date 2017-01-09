---
layout: post
title: 关于Bash Shell
categories: [blog]
tags: [tags]
description: 
---

关于Bash Shell



## 变量

```shell
language="shell"
echo $language
echo ${language}
```

花括号用来区分边界，相当于`()`

```
for language in Java Python Erlang Elixir; do
    echo "I write ${language}'s program"
done
```

变量名和等号之间不能有空格

```
for file in `ls ~/`
```

## String

单引号

str='this is a string'
单引号字符串的限制：

单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
单引号字串中不能出现单引号（对单引号使用转义符后也不行）
双引号

your_name='qinjx'
str="Hello, I know your are \"$your_name\"! \n"
双引号里可以有变量
双引号里可以出现转义字符