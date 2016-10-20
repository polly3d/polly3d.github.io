---
layout: post
title: TypeScript学习笔记
---

项目中需要用到ionic，并且采用的ionic的v2版本。这个版本是基于angularjs的，而且也是v2版本。而v2版本的语法基于TypeScript。所以，最先学习的，就是TypeScript了。

## TypeScript简介

Type + Javascript = TypeScript，即javascript加上类型检测，就是TypeScript了。

## 数据类型
 
一段代码显示所有的数据类型。
```
let isTest: boolean = true;
let decimal: number = 6;
let color: string = "red";

let fullName: string = `Bob Bobbing`;
let sentence: string = `Hello, my name is $( fullname ).`;

let list: number[] = [1,2,3];
let list: Array<number> = [1,2,3];

//Tuple
let x: [string, number];
x = ["hello",10];

//Enum
enum Color {Red, Green, Blue};

//Any
let notSure: any = 4;
notSure = 'may be a string';
noSure = false;
let list: any[] = [1,true,"fee"];

//Void
function warnUser(): void{
    alert("This is a warn");
}
```

同时，还有`null`和`undefined`类型。并且还有一个特殊的类型`never`。函数默认的返回类型就是`never`。

类型转换有以下两种方式：

```
let someValue: any = "this is a stirng";
let strLength: number = (<string>someValue).length;
```

```
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

## 变量

定义变量有以下三种方式:
* var
* let
* const

文档中是推荐使用let，如果定义的变量不改变，推荐使用const。let可以避免比较多的作用域混乱的问题。

## Interface（接口）

TypeScript核心的定义里面就有类型检查，因此涉及到很多接口的问题。TypeScript里面的接口是非常灵活有趣的。在执行类型检查时，只检查最少定义，即接口除了类型定义的内容之外，还可以定义其他其他内容。代码如下：

```
function printLabel(labelObj: {label: string}){
    console.log(labelObj.label);
}
let myObj = {size: 10, label: "Size"};
printLabel(myObj);
```

写成Interface的形式如下：
```
interface LabelValue {
    label: string;
}

function printLabel(labelObj: LabelValue){
    console.log(labelObj.label);
}
let myObj = {size: 10, label: "size"};
printLabel(myObj);
```

Interface可以继承Class，这也是大开眼界了。

## Class（类）

原生js使用function和prototype作为面向对象。而TypeScript使用了更加面向对象的语法，与主流语言类似。代码如下：

```
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```
所有的方法默认是public。同时也支持private和protected。这些修饰符基本与其他语言类似。

另外，class可以使用set和get来进行Accessor。

```
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

## 函数

TypeScript的函数还是和js的函数一样，比较乱，这个需要一点时间来整理，列为todo吧。主要还是要分清楚，this是什么……

