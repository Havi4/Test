# 2.10 函数作为返回值
## 函数作为返回值

如果你从来没有使用过讲函数作为第一类型的语言，那么你需要坐下来，因为我接下来讲的会让你刚到一点晕：在Swift中，函数是第一类公民。这意味着一个函数可以用在任何一个值使用的地方。比如，函数可以赋值给变量；函数可以传递给函数参数；函数可以作为值返回。

函数具有严格的类型。你可以给一个变量赋值，或者给函数输入一个值只要类型是正确的，为了让一个函数作为值，它就必须具有类型。事实上，函数是有类型的？想到什么了？函数的签名就是函数的类型。

使用函数作为一个值的主要目的是在后面这个函数被调用的时候可以不去定义这个函数是什么。
下面是一个简单的例子：
```swift
func doThis(f:()->()){
  f()
}
```
*doThis*函数带有一个参数没有返回值。这个参数*f*是一个函数；因为这个参数的类型不是Int，String，或者Dog，而是一个函数签名，*（）-> ()*.这表明了函数没有参数和返回值--在函数名后面是一个大括号。

你该如何调用这个函数呢？为了这样做，你需要给这个函数传递一个函数作为参数，其中一个方法是给使用函数的名字作为参数：
```swift
func whatToDo() {
  print("i did it")
}
doThis(whatToDo)
```
第一步，我们定义了一个该类型的函数--一个不带参数和返回值的函数。然后，调用*doThis*,把函数名字作为函数的参数。注意这里我们没有将调用*whatTodo*,我们只是传递它。因为在函数名字后面没有大括号。当然这是可以的：我们把*whatToDo*作为*doThis*的参数，*doThis*调用它收到的参数，然后在控制台输出"i did it"。

但是这样做的关键是什么？如果我们的目的是调用*whatToDo*，我们为什么不直接调用呢？在其他例子中，这是没有什么用处的；我仅仅是给你展示下相关的语法和结构。在现实生活中，这是非常有意义的，因为一个函数可能会将一个函数作为参数。比如，他可能需要在做完一些任务后调用某个函数，或者一段时间后。

比如，在一个函数中封装函数调用可以减少重复和出错的机会。下面是我程序的一个例子。在Cocoa中一个常见的例子是，绘制一个图形，这个可以分解为四步：
```swift
let size = CGSizeMake(45,20)
UIGraphicBeginImageContextWithOptions(size, false, 0)
let p = UIBezierPath(roundedRect:CGRectMake(0,0,45,20),cornerRadius:8)
p.stroke()
let result = UIGraphicGetImageFromCurrentImageContext()
UIGraphicsEndIamgeContext()
```

> * 打开一个image上下文
> * 在上下文中绘制
> * 导出图片
> * 关闭图片上下文

上面的操作是非常令人讨厌的。上面代码的最终目的是获得结果，"图片";但是最后的结果隐藏在了其他代码中。同时，整个代码结构也是死板的；每次我在app中这么写，实际都是一样的。更重要的是，如果我忘记了异步，就会发生严重的错误。

每次不同的操作就是我做的第二步。因此，第二部我需要重构出来如果把第二个写成一个工具函数，整个问题就可以解决：
```swift
func imageOfSize(size: CGSize, _ whatToDraw:()->()) -> UIImage {
  UIGraphicBeginImageContextWithOptions(size, false, 0)
  whatToDraw()
  let result = UIGraphicGetImageFromCurrentImageContext()
  UIGraphicsEndIamgeContext()
  return result
}
```
我的*imageOfSize*工具很有效果，我可以把它声明到文件的顶层，这样所有的文件可以看到它。为了绘制一个图片，我在函数中执行第二步，然后把这个函数传递到*imageOfSize*函数中：

```swift
func drawing() {
  let p = UIBezierPath(roundedRect: CGRectMake(0,0,45,20),cornerRadius: 8)
  p.stroke()
}
let image = imageOfSize(CGSizeMake(45,20),drawing)
```
注意到这是一个非常完美的表达式，可以非常清晰的表达出一个image。

在Cocoa的API中，有许多地方你需要以特殊的方式或者某个时间传递一个函数给runtime。比如，当一个控制器展示另一个控制器时，你调用的函数带有三个参数--需要展示的控制器；一个控制弹出控制器模式的bool值；和弹出成功后你需要调用的一个函数：
```swift
let vc = UIViewController()
func whatToDoLater(){
  print("i done")
}
self.presentViewController(vc, animated:true, completion:whatToDoLater)
```
cocoa的文档把这样的一个函数成为句柄，会引用到一个block，因为这是OC语法的需要，在Swift中，这是一个函数，把它看做一个函数。

在Cocoa中的一些场景中，甚至会传递两个函数到一个函数中，比如，当你执行一个动画的时候，你经常会传递一个函数描述动画效果，另一个函数告诉动画结束后的动作：
```swift
func whatToAnimate() {
  self.myButton.frame.origin.y += 20
}
func whatToDoLater(finsished:Bool) {
  print(finished)
}
UIView.animateWithDuration(0.4,animation:whatToAnimate,completion:whatToDoLater)
```

这意思是：改变界面上button的起点y，0.4秒完成。然后完成之后打印一条消息表明是不是完成。


> **指点**
> 为了是函数类型清楚，采用Swift**typealias**的特征创建一个函数类型。名称可以重新定义，可以避免令人迷惑的箭头。比如，你可以给（）-> （）明别名
> typealias VoidVoidFunction = () -> ().然后你可以后面的代码中使用VoidVoidFunction来给函数类型签名。


