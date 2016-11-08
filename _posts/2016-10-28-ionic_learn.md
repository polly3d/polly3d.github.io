---
layout: post
title: Ionic学习笔记
---

# 资源

* [Angularjs 2 各种问题汇总](https://github.com/kittencup/angular2-ama-cn)
* [ionic2+angular2中踩的那些坑](http://www.cnblogs.com/yanxiaodi/p/5750860.html)
* [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
* [ionic+nodejs开发遇到的跨域和post请求数据问题](http://www.cnblogs.com/ytu2010dt/p/5471366.html)
* [ionic开发篇之那些年我们踩过的坑](http://blog.csdn.net/yourlin/article/details/48268361)
* [使用ionic2开发一个登录功能](http://www.cnblogs.com/madyina/p/5970814.html)
* [ionic调试方法总结](http://blog.csdn.net/ioniconline/article/details/50162685)
* [Build a Mobile App with Angular 2 and Ionic 2](https://scotch.io/tutorials/build-a-mobile-app-with-angular-2-and-ionic-2)
* [Exploring Nav Hierarchy in the Ionic 2 Tabs Page](https://webcake.co/exploring-nav-hierarchy-in-the-ionic-2-tabs-page/)
* [Using Cordova Plugins in Ionic 2 with Ionic Native](http://www.joshmorony.com/using-cordova-plugins-in-ionic-2-with-ionic-native/)
* [所有的@type文件地址](https://www.npmjs.com/~types)
* [你不懂JS: 异步与性能 第一章: 异步: 现在与稍后](http://www.jianshu.com/p/8b985ea85e30)
* [JavaScript函数式编程](https://zhuanlan.zhihu.com/p/21714695)


# 开发中的坑

## Ionic中，如何做非父子关系的界面跳转？

正常情况下，Ionic界面跳转有如下方式：
* 使用`ion-nav`组件，设置root。
* 使用`ModelController`，以弹出窗口的形式，展示界面
* 使用`NavController`，push、pop的方式进行前进或回退到某个界面，右上角可以有一个back按钮


以上，基本有一个父子或顺序界面的概念。如果从一个界面跳转到另外一个界面呢？
答案是使用Service。
见代码：
```
//service
export class NavTestService {
    change: EventEmitter<string>;

    constructor(){
        this.change = new EventEmitter();
    }
}

//loginPage to startPage
gotoStartPage(): void {
    this.navService.change.emit('start');
}

//root component
ngAfterViewInit(): void {
    this.navService.change.subscribe((page: string) => {
        console.log(page);
        this.changeRootPage(page);
    });
}
```

## Ionic的Menu，如果有界面覆盖其上时，比如Model，也能够执行滑动

## 调用接口时，如果api不是接受json格式，会引发重定向的问题。

需要进行如下转换：
```
login(): void {
    let headers: Headers = new Headers({'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8'});
    this.http.post('http://api','username=dd&password=dd',{headers: headers})
        .subscribe((response) => {
            console.log(response);
        },(error) => console.log(error),() => console.log('complete'));

}
```

* 增加header
* 将数据转成`&`连接格式


## 打包android的基本步骤

准备工作：

* 安装android SDK。都在[资源](http://www.androiddevtools.cn/)这里，不要再去满世界找了。
* 安装或升级或，要弄清SDK版本。

模拟器运行：

* 运行命令`ionic platform add android`，增加andorid平台;
* 将`%项目位置%\platforms\android\project.properties`中的target改为对应SDK版本；
* 将`%项目位置%\platforms\android\CordovaLib\project.properties`中的target改为对应SDK版本，版本号需要与上一步相同；
* 运行命令`ionic emulate andorid`，运行模拟器。

*特别说明：如果Android SDK 版本安装不正确，会有一堆的坑。而且，在使用命令行打包是时，需要下载gradle。这个过程很慢，但也有解决办法。虽然网上有解决办法，但下载完gradle后，还是会下载一堆依赖。所以，最好的办法，还是翻出去，这样网络会好一点*

## 调试方法

### 使用chrome的inspect调试

* 打包Android的apk文件，并安装到手机。（模拟器操作相同）
* 打开手机的开发者模式，访问电脑chrome的 chrome://inspect ， 点击要inspect的app，会打开熟悉的chrome developer tool.
* 点击inspect，打开chrome debugger tool.

## phpstorm突然提示找不到某个东西（类），无法编译

注：这个只出现在phpstorm（IDE）里面。在命令行下不会有这样的问题。

比如，使用Promise的时候，提示*can't find name Promise*。这个是由于新版本所有的内容都使用@type了。不再使用typings。
具体见：[文档](https://github.com/driftyco/ionic/blob/master/CHANGELOG.md#typings)

造成这种问题，主要是phpstorm里面无代码提示，无法编译。
解决办法：
* 在phpstorm里面进行设置。settings -> Languages & Frameworks -> JavaScript -> set language。将language设置为es6。




