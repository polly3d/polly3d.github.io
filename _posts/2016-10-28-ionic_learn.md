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




