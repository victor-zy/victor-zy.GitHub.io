## Welcome to Victor-zy  blog

##Python 正则表达式随便讲讲
###什么是正则表达式？

[TOC]
正则表达式，又称正规表示式、正规表示法、正规表达式、规则表达式、常规表示法（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些匹配某个模式的文本。
许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在Perl中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由Unix中的工具软件（例如sed和grep）普及开的。正则表达式通常缩写成“regex”，单数有regexp、regex，复数有regexps、regexes、regexen。
引用自维基百科https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F
###第一个例子，hello world
```
<html><body><h1>hello world<h1></body></html>
```
我们提取出hellow world，大部份人的第一想法是不是这样：
```
s = <html><body><h1>hello world<h1></body></html>
start_index = s.find('<h1>')
```
我室友昨天晚上就这样入坑的，当时我也没怎么细看。这样不是不可以，但很麻烦，万一你有多个标签，一不小心就匹配到了很多的东西，准确的匹配还要写多层循环，一个字，烦。
####那我们要怎么处理这个情况呢？
```
import re
key = r"<html><body><h1>hello world<h1></body></html>"#这段是你要匹配的文本
p1 = r"(?<=<h1>).+?(?=<h1>)"#这是我们写的正则表达式规则，你现在可以不理解啥意思
pattern1 = re.compile(p1)#我们在编译这段正则表达式
matcher1 = re.search(pattern1,key)#在源文本中搜索符合正则表达式的部分
print matcher1.group(0)#打印出来
```
先来做个test， 你要匹配到一个字符串里面的所有 “hellow”，你怎么做？
```
import re
key = r"源文本"#这是源文本
p1 = r"hellow" #这是我们要写的正则表达式
pattern1 = re.compile(p1)#同样是编译
matcher1 = re.search(pattern1,key)#同样是查询
print matcher1.group(0)
```

####开始讲解
1: python和正则表达式都区分大小写，所以"Python"和"python"是不一样的，要是在匹配的时候，大家要注意这点，不然就匹配不到了。
2:那么刚刚那个```<h1>hello world<h1>```怎么写？
```
import re

key = r"<h1>hello world<h1>"#源文本
p1 = r"<h1>.+<h1>"#我们写的正则表达式，下面会将为什么
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
``` . ```字符在正则表达式代表着任何一个字符，也包括他自己
```findall```返回的是所有符合要求的元素列表，包括仅有一个元素时，它还是给你返回的列表。
当然了，这里嘛还有一个坑，就是转义字符，我也讲的不太清楚，大家可以询问

**黄索图**（这是重点 ）

###重点在上面 

```+```的作用是将前面一个字符或一个子表达式重复一遍或者多遍。
例如表达式“ab+”那么它能匹配到“abbbbb”，但是不能匹配到"a"，它要求你必须得有个b，多了不限，少了不行，在那么都是不会满足的，你会问我“有没有都行，有多少都行的表达方式”，黄素图会告诉你，有的。

```*```跟着其他符号后面，可以匹配到它多次
比如：我们在其他地方匹配到有的是”http“或者”https“怎么棒？
```
import re

key = r"http://www.426.com and https://www.ysy.com"#颜速毅专属网址
p1 = r"https*://"#看到那个星号没有！星号****
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
输出的效果：
```
['http://', 'https://']
```

3:多字符匹配方式:```[]```
有的反爬技术，”我瞎jj的“，会在```<html></html>```类似的标签上，大小写乱来，我们怎么做？
```
import re

key = r"lalala<hTml>hello</Html>heiheihei"
p1 = r"<[Hh][Tt][Mm][Ll]>.+?</[Hh][Tt][Mm][Ll]>"
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
```[^]```代表除了内部包含的字符以外都能匹配
```
import re

key = r"mat cat hat pat"
p1 = r"[^p]at"#这代表除了p以外都匹配
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
####还有这些 
| 正则表达式 | 匹配字符 |
|-----|-----|
|[0-9] |0123456789任意之一 | 
| [a-z] |小写字母任意之一  | 
| [A-Z] |大写字母任意之一  | 
| \d | 等同于[0-9]  | 
| \D | 等同于[^0-9]匹配非数字  | 
| \w | 等同于[a-z0-9A-Z_]匹配大小写字母、数字和下划线   | 
| \W |等同于[^a-z0-9A-Z_]等同于上一条取非  | 

4:在生活中，我们经常会遇到这些问题
```
import re

key = r"victor-zy@.github.io"
p1 = r"@.+\."#
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
输出
```
.github.io
```
我理想的是：```.github```
因为正则表达式默认是“贪婪”的，我们之前讲过，“+”代表是字符重复一次或多次。但是我们没有细说这个多次到底是多少次。所以它会尽可能“贪婪”地多给我们匹配字符，在这个例子里也就是匹配到最后一个“.”。
我们怎么解决这种问题呢？只要在“+”后面加一个“？”就好了。
```
import re

key = r"victor-zy@.github.io"
p1 = r"@.+?\."#我想匹配到@后面一直到“.”之间的，在这里是hit
pattern1 = re.compile(p1)
print pattern1.findall(key)
```
输出结果：
```
['@.github.']
```
 /categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
