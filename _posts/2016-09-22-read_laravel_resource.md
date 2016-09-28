---
layout: post
title: 【Laravel源码阅读】
---

纠结好比较长一段时间，有好些问题没有解决。有些问题大，比如如何开发一个扩展性强的系统。有些问题小，比如为什么要（或不要）使用repository模式。众说纷云，很容易就迷失在资料的海洋里，无法自拔，导致浪费了很多时间。
因此，不再纠结这些问题了。直接就从Laravel的源码开始看起。根据Laravel的实现原理，一边复习和熟练PHP的众多特性；一边编写简单的示例框架，比如写个权限系统、项目管理系统、工单系统、购物系统等。
相信不断的尝试和总结，总会有所收获。

## 从最简的web应用过程开始

使用最简单的步骤描述一个web应用过程，应该是这样的：

* 用户输入web应用的地址
* web应用处理数据并返回
* 载体（浏览器）呈现结果

那么作为一个web应用所要做的，就是处理数据并返回。由此，可以推断出最简洁的web应用程序的过程。

* 捕获输入的数据（HTTP传递的数据）
* 创建web应用实例
* 使用web应用处理数据
* 返回结果

```
<?php

$app = new Application();
$request = $_REQUEST;
$response = $app->handler($request);
return $response;

```

这就是一个最简单应用的处理过程，这也是Laravel入口文件index.php里面的基本流程内容。这样处理非常的简洁，而且非常的优雅。

那么Application是一个什么样的类呢？
如何处理用户的请求呢？
处理完成之后，返回什么，按什么样的格式呢？
每一个请求是否需要单独一个php文件呢？
项目结构应该怎么样组织才更合理呢？

看看Laravel是怎么处理的。

```
===== Laravel's index.php ====

<?php
require __DIR__.'/../bootstrap/autoload.php';
$app = require_once __DIR__.'/../bootstrap/app.php';
$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);
$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
);
$response->send();
$kernel->terminate($request, $response);
```
