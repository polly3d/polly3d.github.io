---
layout: post
title: 【octoberCMS】记录
---


## 开发插件

### 使用Builder

* Create plugin，输入插件名称
* 添加Database
* 添加Models，并添加对应的Forms和Lists
* 添加Backend Menu。暂时可以不填写menu的url
* 添加Controllers。
* 返回Backend Menu，添加menu的url。

## Form使用

### 使用relation

方法一：配置字段

* 在fields.yaml里面填写相应的type。如下

```
fields
    comments:
        lable:评论
        type:relation
        nameFrom: content
```

* 修改对应的model里面对应的关系，通过octoberCMS通过属性来定义关系。注：Laravel是使用方法定义的。
* 如上，增加nameFrom。nameFrom指的是显示哪个字段。
* 也可以使用select属性。select属性是自定义选择不同的列。如`select: sql语句`。

方法二：使用Behaviors里面的RelationController

todo


### 如何增加type类型(比如添加格式化功能)

With form widgets you can add new control types to the back-end forms. They provide features that are common to supplying data for models. Form widgets must be registered in the Plugin registration file.

方法一：建立新的formWidget

* 创建formWidget类。所创建的类必须在某个Plugin的命名空间下，并且继承`Backend\Classes\FormWidgetBase`类。
* 主要方法如下：
```
/**
 * Initialize the widget, called by the constructor and free from its parameters.
 */
public function init()
{
    
}

/**
 * Renders the widget.
 * @return view
 */
public function render()
{
    
}

/**
 * Prepares the view data
 */
protected function prepareVars()
{
    
}
```

* 创建视图。视图文件夹规则如下：
    - 与formWidget类同层目录；
    - 文件夹与formWidget同名，并在同名文件夹下建立如_viewname.htm的文件。
    - formWidget根据需要，渲染对应的视图。


* 最后一个步骤，就是将formWidget注册到Plugin里面。

```
public function registerFormWidgets()
{
    return [
        'Wang\Demo\FormWidgets\Currency' => [
            'lable' =>  'my currency',
            'code'  =>  'currency'
        ],
    ];
}
```


最方便的办法，使用命令行。

```
create:formwidget
```

方法二：重写ForController


## 记录

* widget分为三类
    - 普通widget
    - formWidget，主要用于form表单
    - reportWidget，主要用于报告

## 扩展Builder插件


