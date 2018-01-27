---
layout:     post
title:      "Markdown 学习"
subtitle:   "Markdown 常用指令 "
date:       2018-01-27
author:     "Xiaotong CHEN"
header-img: 
tags:
    - Markdown
    - post
---

# Markdown 学习
一个好的Markdown学习网站
http://xianbai.me/learn-md/index.html

## 注释comment
\<!-- 注释 -->

---
## 标题Header
\# 加空格实现

---
## 句段Sentence/Paragraph
### 换行 
- 行末加两个空格 
- <br>

---
## 符号Punctuation Charcaters
- \*倾斜* 
- \-或+ 列表
- \>引用
### 字符实体
    tab 预格式化 

---
## 分隔符Horizontal Rules
\--- 三个减号或星号或下划线需隔开上一行 

---
## 文本格式Text Styling
- 星号（*）或下划线（_）包围的文字将会显示斜体 ex: *倾斜*
- 两个星号（**）或下划线（__）包围的需要特别强调的文字将会加粗 ex: **加粗**
- 两个等号（equalsigns：=）包围来突出高亮显示。ex: ==高亮==
- 使用两个加号（plus sign：+）来标记下划线。
ex：++下划线++
- 两个波浪符号（two wavy line:~~）包围来给文本添加删除线。ex: ~~删除线~~

---
## 脚标 Script
标准 Markdown 不支持脚标，只能通过内嵌 HTML 的<sup>和<sub>标签来实现。
* 脚标两边加上< > ex: 2^10^ 
* 脚标两边加上~ ~ ex: H~2~o

---
## 链接Hyperlink
### 文字
- Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要用尖括号包起来的文字， Markdown 就会自动把它转化成链接。如果你还想要加上链接的 title ，只要在网址后面用双引号把 title 文字包起来即可。
- \[text](url"解释“) ex: [链接](href: "this is a null ref")
- 先定义参考refid：[text][refid]
再定义refid所指：[refid]:URL
### 图片
1. 插入图片
-  需要在链接文字方括号之前添加一个感叹号（exclamation mark：!），其语法格式为     \!\[alt_text](url) 其中alt_text可以置空    
- **Markdown 中的段落（包括图片）默认顶格左对齐，若要将图片居中，可以直接内嵌 HTML 的 <img> 标签，设置align="middle"。如果还不行，可以尝试封裹一层 div 设置 style="text-align:center" 实现：**
2. 图片链接
- 我们在 Markdown 图片标记![]()外面再嵌套一层[]()即可建立图片超链接，点击图片即可跳转到链接地址。
图片链接的格式看起来大概是这样的:
\[\!\[](img_url)](ref_url)

---
## 锚点inner link
### 书签Bookmark
    先定义锚点id：<a href="#auchor_id">bookmark_text</a>
    再定义一个id为auchor_id的对象（这里以<p>为例）：<p id="auchor_id">auchor_text</p>
### 脚注Footnote
    先在需要脚注的单词（terminology）后面添 加 [^Footnote] ： terminology[^Footnote]
    再在文末 glossary 区域定义脚注（添加注解）： [^Footnote]：explanatory notes

---
## 引用Blockquote
Markdown 标记区块引用是使用类似 email 的引用方式，在断好的行前加上 > （more than or greater than sign）：

---
## 代码Code
### 行内代码Inline Code
Use the `printf()` function.(此处使用了反斜杠转义)
### 代码块Code Blocks
- 可使用预格式化引用语法格式。Preformatted Code Block
在句段的行首插入1个 tab 或4个空格，则表示代码块。
- [Fenced Code Block]
在句段行首和行末用三个反引号换行闭包，并在行首三个反引号后添加 YAML 语言标识。
---
## 列表List
### 无序列表Unordered List
无序列表（unordered, bulleted）项目的行首使用星号（或加号，或减号）加空格作为列表标记list markers
### 有序列表Ordered List
有序列表（ordered / numbered）项目的行首则使用数字接一个英文句点标记（use numbers followed by periods）
### 任务列表（Task Lit）
GFM 扩展支持把列表变成带勾选框的任务列表，只需要在列表标记后添加[ ]标记☐表示unchecked，在中括号中填写x（[x]）标记☑︎表示checked（filled）。

---
## 表格Table
You can create tables by assembling a list of words and dividing them with hyphens - (for the first row), and then separating each column with a pipe | (vertical bar):
==**表格仍有问题**== 
