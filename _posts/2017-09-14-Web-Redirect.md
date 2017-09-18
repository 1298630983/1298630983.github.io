---
layout: post
title:  "重定向"
categories: Java Web
tags: web
author: Jason
excerpt_separator: "!"
---

* 目录
{:toc}

# 一.重定向
![](http://a2.qpic.cn/psb?/V141zJkM4D6xvi/jB*bX17OuXlJJ.D*xzhEj2CO8C2iWj2LnNAmuFzMp0E!/b/dBkBAAAAAAAA&ek=1&kp=1&pt=0&bo=JQS9BSUEvQUDFzI!&vuin=1298630983&tm=1505710800&sce=60-4-3&rf=viewer_4)

# 二.访问路径
## 1.如何获取访问路径
- 项目名:getContextPath
- Servlet访问路径:getServletPath
- URI:getRequestURI()
- URL:getRequestURL()

## 2.URI和URL的区别
### 1)狭义的理解(Java Web项目)
- URI:绝对路径
- URL:完整路径

### 2)广义的理解(任何Web项目)
- URI:资源的名字
- URL:资源的真名
> URI包含URL

## 3.如何配置Servlet访问路径
### 1)精确匹配
- /hello
- 必须通过/hello才能访问此Servlet
- 此Servlet只能处理/hello一个请求

### 2)通配符
- /*
- 通过任何路径都可以访问此Servlet
- 此Servlet可以处理一切请求

### 3)后缀
- *.abc
- 以".abc"为后缀的请求都能访问此Servlet
- 此Servlet可以处理多个请求


## 4.如何使用一个Servlet处理多个请求?
![](http://a3.qpic.cn/psb?/V141zJkM4D6xvi/IRpFXO7*d.ueefLr.LZ5G3YsNNB4plN5BIVZSG9Ej1w!/b/dAEBAAAAAAAA&bo=hAO6AoQDugIDACU!&rf=viewer_4)

# 三.Servlet生命周期
![](http://a1.qpic.cn/psb?/V141zJkM4D6xvi/C1R4WB6kf95vdFUlglsrmMexm.kZqstFRN6kzJlttl0!/b/dEIAAAAAAAAA&bo=3AMjA9wDIwMDACU!&rf=viewer_4)

# 四.ServletConfig和ServletContext
## 1.它们的作用
-用来读取web.xml中为Servlet预留的参数
![](http://a1.qpic.cn/psb?/V141zJkM4D6xvi/GUr7frcUn8m5XjBlq8cxKLtw6qHNU0xnNxn3Ds7oXmQ!/b/dBoBAAAAAAAA&bo=ZwRbA2cEWwMDACU!&rf=viewer_4)

## 2.它们的区别
- config和servlet是1对1的关系
- context和servlet是1对多的关系
- 若数据只是给某个servlet使用,则用config
- 若数据给多个servlet使用,则使用context
> 它们的关系由服务器来保障

## 3.config使用场景
- 假设开发一个网页游戏,若超过人数上限则要排队
- 开发登录功能LoginServlet
- 人数上限应该是一个可配置的参数maxOnline
- 该参数由LoginServlet使用
>由于该参数只是LoginServlet使用,由config读取即可

## 4.context使用场景
- 大部分查询数据都具有分页功能
- 分页需要一个参数:每页显示多少条数据 size
- 该参数一般可以配置,由于被众多查询功能复用,使用context存取

## 5.context可以存取变量
![](http://a3.qpic.cn/psb?/V141zJkM4D6xvi/z6hl*VCwk6ixxYGd7cZkYTRILtHovQSISAEG6SWosec!/b/dAEBAAAAAAAA&bo=VgTfAlYE3wIDACU!&rf=viewer_4)

