---
layout:     post
title:      "WEB学习-HTML和CSS入门"
subtitle:   "Udacity-WEB学习 "
date:       2018-02-20
author:     "Xiaotong CHEN"
header-img: 
catalog:
tags:
    - HTML
    - CSS
    - WEB入门
---

* 目录
{:toc}

# WEB项目文件结构
![文件结构](/img/post/WEB-structure.png)

# 常用网页框架设计
- 1个100%页面宽度的行类(.row)
- 占页面1/12-12/12的列类(.col-n)*n为1-12的数字*
*CSS类前有.*

# flex
- 为了让行中列元素并排显示，需要给row加上`display:flex`
- 关于flex，查看[flex](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex),[flexbox](https://segmentfault.com/a/1190000007550042),[flex-wrap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-wrap)

# 负空间（space）
- margin 外边距
- padding 内边距
- 加上后缀`-top -right -left -bottom`具体设置
*border 边框*

# 溢出
- `overflow:auto;`

# 响应
例子：
`@media only screen and (max-width:500px) and (orientation:landscape)`
- `@media`实现页面响应
- `only‘`用于兼容旧式浏览器
- `screen`指示这个响应用于所有显示设备
*print用于响应当用户要打印页面时*
- `and`加上条件
- `orientation`指的是手机方向：landscape mode横向，portrait mode纵向

# 浏览器兼容
在head中添加normalize.css样式表，该表格google搜索下载

# 占位图片
- `<img src="http://placehold.it/nxn"`
- `<img src="http://placekitten.com/n/n"`

# 字体
1. 在[Google Fonts](https://fonts.google.com/)中找到字体
2. 在html的head中加入链接`<link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet">`
3. 在css中为需要使用该字体的元素添加`font-family: 'Roboto', sans-serif;`


