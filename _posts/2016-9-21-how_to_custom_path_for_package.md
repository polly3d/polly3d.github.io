---
layout: post
title: composer自定义package包安装路径
---

# 原文

地址：https://getcomposer.org/doc/faqs/how-do-i-install-a-package-to-a-custom-path-for-my-framework.md

# 译文

每个框架可能需求有一个或多个安装路径不同的依赖包。通过使用composer/installers，Composer能够把包安装到除vendor外的其他文件夹里面。

如果你是包的作者，并且你想把包安装到指定路径，最简单的办法就是`require composer/installers` 并且设置合适的`type` 属性。对于类似CakePHP，Drupal 和 WordPress等框架，这是非常普通的需求。下面以WordPress主题的一个composer.json文件为例：

```
{
    "name": "you/themename",
    "type": "wordpress-theme",
    "require": {
        "composer/installers": "~1.0"
    }
}
```

当使用Composer安装这个主题时，将会安装到`wp-content/themes/themename/`文件夹中。目前直接类型，请点击[查看](https://github.com/composer/installers#current-supported-types)。

使用额外的`installer-paths`配置，自定包可以定义或重写依赖包的安装路径。一个有用的例子，Drupal配置多站点时，需要将包安装到站点的子目录中。现在，我们使用`compser/installers`为模块重写安装路径：

```
{
    "extra": {
        "installer-paths": {
            "sites/example.com/modules/{$name}": ["vendor/package"]
        }
    }
}
```

现在，依赖包就不会安装在`composer/installers`指定的位置，而是安装到你指定的文件位置中了。