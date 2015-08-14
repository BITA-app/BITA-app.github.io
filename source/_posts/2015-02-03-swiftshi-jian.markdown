---
layout: post
title: "swift实践：开始swift"
date: 2015-02-03 20:38:25 +0800
comments: true
categories: 
---

###引言
声称一种语言优于另一种语言被认为是不礼貌的行为。但是，没有一个编程语言的设计者会相信“不同的语言各有千秋”这种文绉绉的客套话。                                                    ————《黑客与画家》

oc在语言排行榜上的位次逐月下降，而swift在逐月上升。相信swift取代oc的地位不会太久远。开始学习和使用swift吧，至少可以通过swift这扇窗，接触很多其它语言的编程思想。
###新建项目

1、在iOS开发领域，swift语言只支持iOS7.0以上的系统。
	
2、新建一个swift语言项目,和oc项目没有太多区别，简单看一下：

*	.h和.m文件现在是一个.swift文件。
*	项目中找不到main函数入口。AppDelegate文件中，在类体之前有一行`@UIApplicationMain`标记，这个标记做的事情就是将被标注的类作为委托，去创建一个 UIApplication 并启动整个程序。在编译的时候，编译器将寻找这个标记的类，并自动插入像 main 函数这样的模板代码。
*	oc中引入头文件，在swift中对应引入模块。`#import <UIKit/UIKit.h>`只需写成`import UIKit`。
*	除引入其它模块外，同一模块内的类之间相互调用不需要显式引用。一般一个target对应同一个module。不同的静态库对应不同的module，所以引入静态库时要import相应的module name。工程文件target设置中有Product module name一项。这一module name在使用oc和swift混编时需要用到。
*	AppDelegate类的声明，`class AppDelegate: UIResponder, UIApplicationDelegate`，可以看出，继承的类和遵守的协议，写法上没有什么区别。和oc一样，swift不支持多继承，但是可以通过协议的方式达到类似的效果。
*	和oc不同的是，类不必都派生于一个NSObject一样的基类，你可以实现自己的基类。从这点说，swift更开放，我们可以期待swift在更多领域大展身手。另外，这点和cocoa框架有些矛盾，cocoa框架下所有对象都继承自NSObject类。这在oc和swift混编时会有一些限制，只有继承自NSObject类型的swift类才能在oc中使用。
*	AppDelegate中的属性声明：`var window: UIWindow?`，属性和成员变量，不像oc那样区分，不需要加`@property`修饰符。类中最外层（不在方法体内）声明的常量和变量，都是属性，可以通过.去获取。属性有访问级别的限制，分为public、internal、private三种，默认是internal，即可以被自己模块或应用中被访问到。
*	AppDelegate中方法声明：
```
func application(application: UIApplication,didFinishLaunchingWithOptions launchOptions:[NSObject:AnyObject]?) -> Bool 
```，可查看swift语言说明，详不赘言。
	
*	单行语句末尾不必加分号。
		
3、
	接下来应该怎么写呢？

###UIViewController

1、viewDidLoad方法，子类继承父类的方法，需要加override关键字，否则编译器会认为你定义了重名的方法，会报错。

2、如果要在view上添加一个tableView：

```
let tableView = UITableView(frame: self.view.frame)
tableView.backgroundColor = UIColor.redColor()
self.view.addSubview(tableView)	
```
生成一个UITableView的实例，不需要经过alloc、initWithFrame等方法调用，而是直接使用UITableView的初始化器（initializer）。如不需要设定frame，可以直接写成UITableView()。初始化器（initializer）和方法不同，init属于关键字，非方法名。因此不需通过对象调用init,而是在类名后加括号，括号内指定参数。

方法调用使用.符号，更接近其它语言的语法。
	
3、从nib中添加一个IBOutlet属性：	
```
@IBOutlet var scrollView : UIScrollView!
```	!表示强行拆包，也就是说这个变量一定要有非nil的值。详情可查阅swift语言中optional变量和?、!的说明。	
	添加一个IBAction方法：	
```
@IBAction func didClickButton(sender:UIButton){
}
```

4、如需打印log，可以使用println函数。如`println("tableview frame:\(tableView.frame)")`其中，使用`\(variable)`的方式可以将变量格式化插入到字符串中。

###和oc混编
1、swift中引用oc类：	
如果是swift项目，初次创建oc文件，会有提示创建一个桥接头文件，只需将需要引用的oc类的头文件在桥接文件中引入即可。

更一般的，可以在工程文件中，选中相应的target，在Build Settings一栏中搜Objective-C Bridging Header一项，指定一个头文件，并在这个头文件中import需要引用的oc类的头文件即可。

2、oc中引用swift类：	
更简单，只需先找到你要引用的swift类所在的module名。一般在target的Build Settings中，搜索Product Module Name一项。比如我查到这一项的配置是YCApp，然后在需要引入swift类的oc文件中，import "YCApp-swift.h"即可，这样这个module下所有的类（有一个限制，那就是继承自NSObject的类，否则oc不能识别。）都可以在这个oc文件中使用了。这个头文件在工程中找不到，但是按cmd并点击可以看到它的内容。clean之后重新编译会重新生成。so easy。