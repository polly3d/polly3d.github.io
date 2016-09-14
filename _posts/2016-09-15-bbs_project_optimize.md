---
layout: post
title: 【Laravel分享】一步一步构建类PHPHub网站：优化篇
---

系列文章索引：

* [【Laravel分享】一步一步构建类PHPHub网站：开始TDD吧！]()
* [【laravel分享】一步一步构建类PHPHub网站：先定个小目标]()
* [【laravel分享】一步一步构建类PHPHub网站：加入用户认证]()
* [【laravel分享】一步一步构建类PHPHub网站：模仿PHPHub]()
* [【laravel分享】一步一步构建类PHPHub网站：重构前端显示]()
* [【laravel分享】一步一步构建类PHPHub网站：优化篇]()


经过前面几轮的TDD，已经大概做出一个类似PHPHub的东西。虽然不是很完整，比如说没有后台，比如说很丑，但是，最起码的功能已经有。如果继续往上面做，就是增加功能的事情。而且，因为写这个程序的目的就是使用TDD，现在大概的目标也达到。所以，就功能上来说，就到此为止了。接下来，就对现有的内容，进行一个简单的优化。看看就Laravel到底有多快。

# 系统配置

## host主机配置 
CPU：i5 3.1GHz 4核
内存：8G
系统：win7 64位

## 虚拟机硬件配置
CPU：1核
内存：1G

## 系统配置：
L:Linux Centos 7.0
N:nginx 1.8.0
M:mysql 5.6.23
P:php 5.6.9

# 优化步骤

* 使用Laravel自身提供的一些方法进行优化
* 开启OPcache
* 使用redis缓存

# 未做任何优化前：

* 首页：124ms~141ms
* Post目录：116ms~284ms
* 单个Post：100ms~135ms
* 教程分类：120ms~157ms
* 用户中心：104ms~176ms

# 【步骤一】Laravel自身的优化

## 优化方法

* artisan config:cache
* artisan route:cache
* artisan optimize

## 优化后数据

* 首页：115ms~140ms
* Post目录：106ms~120ms
* 单个Post：86ms~113ms
* 教程分类：110ms~120ms
* 用户中心：91ms~116ms

# 【步骤二】开启OPcache

## 优化后数据

* 首页：59ms~63ms
* Post目录：47ms~64ms
* 单个Post：37ms~44ms
* 教程分类：45ms~61ms
* 用户中心：44ms~59ms

# 【步骤三】使用redis

这个涉及到比较多的问题，暂时就先不写了，以后另外发文章来讨论这个问题。就根据上面的结果来说，在开启OPcache之后，基本能够满足日常的需求了。


# 总结

* Laravel框架真的是可以很快。所以不用再纠结框架很大，会不会很慢的问题。这些问题都是有解决方案的。一点点的性能牺牲，换来各种便利，非常值得。
* nginx比apache快。这个有待证明。毕竟本机用wamp运行速度没有这么快，一放到lnmp平台上，瞬间就快了起来。这个应该是有很多方面原因的。这个就算是在这里挖个坑，另择吉日再来分析分析。