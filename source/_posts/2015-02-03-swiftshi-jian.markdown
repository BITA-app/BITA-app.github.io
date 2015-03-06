---
layout: post
title: "swift实践"
date: 2015-02-03 20:38:25 +0800
comments: true
categories: 
---

#开始swift

###新建项目
1.swift语言只支持iOS7.0以上的系统。

2.新建一个swift语言项目,和oc项目没有太多区别，简单看一下：

* .h和.m文件现在是一个.swift文件。
* oc中引入头文件，在swift中对应引入模块。`#import <UIKit/UIKit.h>`只需写成`import UIKit`。
* 项目中找不到main函数入口。AppDelegate文件中，在类体之前有一行`@UIApplicationMain`标记，这个标记做的事情就是将被标注的类作为委托，去创建一个 UIApplication 并启动整个程序。在编译的时候，编译器将寻找这个标记的类，并自动插入像 main 函数这样的模板代码。
* AppDelegate类的声明，`class AppDelegate: UIResponder, UIApplicationDelegate`，可以看出，继承的类和遵守的协议，写法上没有什么区别。和oc不同的是，类不必都派生于一个NSObject一样的基类，你可以实现自己的基类。和oc一样，swift不支持多继承，但是可以通过协议的方式达到类似的效果。
* AppDelegate中的属性声明：`var window: UIWindow?`，属性和成员变量，不像oc那样区分，不需要加`@property`修饰符。类中最外层（不在方法体内）声明的常量和变量，都是属性，可以通过.去获取。属性有访问级别的限制，分为public、internal、private三种，默认是internal，即可以被自己模块或应用中被访问到。
* AppDelegate中方法声明：
```
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool 
```，可查看swift语言说明，详不赘言。
* 单行语句末尾不必加分号。

3.接下来应该怎么写呢？

###UIViewController
1.viewDidLoad方法，子类继承父类的方法，需要加override关键字。

2.如果要在view上添加一个tableView：
```
let tableView = UITableView(frame: self.view.frame)
tableView.backgroundColor = UIColor.redColor()
self.view.addSubview(tableView)
```
生成一个UITableView的实例，不需要经过alloc、initWithFrame等方法调用，而是直接使用UITableView的构造器（initializer）。如不需要设定frame，可以直接写成UITableView()。
方法调用使用.符号，更接近其它语言的语法。

3.从nib中添加一个IBOutlet属性：
```
@IBOutlet var scrollView : UIScrollView!
```
!表示强行拆包，也就是说这个变量一定要有非nil的值。详情可查阅swift语言中optional变量和?、!的说明。


添加一个IBAction方法：
```
@IBAction func didClickButton(sender:UIButton){
}
```

4.如需打印log，可以使用println函数。如`println("tableview frame:\(tableView.frame)")`其中，使用\(variable)的方式可以将变量格式化插入到字符串中。
