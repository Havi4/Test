# 3.2 变量声明
## 变量声明
正如第一章解释的，可以使用*let*或者*var*声明：
* *let*声明的是一个常量--在第一次赋值（初始化）后不能改变它的值。
* *var*声明的是一个变量--它的值在赋值后可以改变。

但是，一个变量的类型永远不可以改变。使用*var*声明的变量可以有不同的值，但是value的值的类型必须和定义的一致。因此，一个变量一旦声明，它就必须有明确的类型。你可以显示的或者隐式的给一个变量指定类型：

* 显示的声明变量类型
  在声明的变量后面，增加一个冒号和变量类型的名字：```swift var x: Int```
* 通过初始化隐式的声明
  如果你把初始化作为声明的一部分，并且没有显示的提供类型，Swift可以推导出它的类型，根据后面初始化的值：```swift var x = 1 //x```
  
显示的声明一个变量，然后给这个变量进行初始值是更好的，在一行中定义：
```swift
var x : Int = 1
```
在上面的例子中，显示的声明是类型是多余的，因为它的类型已经从初始值中推导出来。但是有时候，提供类型不是多余的，即使从初始值推断出类型，下面就是几种情况：
* Swift的推断会出错

  在我写代码的过程中，经常会出现当我提供一个*numeric*类型的初始值。Swift会推导为Int或者Double，这根据这个值是不是包含小数点。但是有很多*numeric*类型，比如：
  ```swift
  let separator: CGFloat = 2.0
  ```
  
* Swift不能推导出类型

  在这个情况下，显示变量类型就是Swift根据初始值推断出的类型，一个非常常见的情况就是包含options set类型。
  ```swift
  var ops = [.Autoreverse, .Repeat] //编译错误
  ```
这个问题的愿意是名字".Autoreverse"和".Repeat"是UIViewAnimationOptions.Autoreverse和UIViewAnimationOptions.Repeat的缩写，但是Swift不知这个类型，除非我们告诉它:
```swift
let opts :UIViewAnimationOptions = [.Autoreverse, .Repeat]
```
* 程序猿不能推断出类型

  我经常显示的写出变量类型，作为对自己的一个提醒。下面是我代码的例子：
  ```swift
  let duration:CMTime = track.timeRange.duration
  ```
  在上面的例子中，track是AVAssetTrack.Swift知道*duration*是AVAssetTrack中的一个属性。但是我不清楚，为了提醒我自己，我显示的写出变量的类型。
  
因为可以显示的声明变量类型，所以当一个变量声明的时候可以不进行初始化。下面的写法也是合法的：
```swift
let x : Int
```
现在x只是一个空的盒子--一个Int类型的变量但是没有初始值。我可以认真的告诉你，如果你可以提供初始值的话要避免这样做。这不是什么灾难--Swift编译器会在你使用一个没有初始值的变量的时候告诉你--但是这个不是好的习惯。

有一个例外就是，我们称为条件初始化。有时候，我们一开始并不知道变量的初始值，直到我们经过了一些条件后。但是，这个变量只能够声明一次；所以只可以变量提前声明，然后是条件初始化。这个方法是没有合理解释的：
```swift
let timed: Bool
if var == 1 {
  timed = true
}else {
  timed = false
}
```
当一个变量的地址作为函数的参数传递的时候，变量必须提前声明和初始化，即使初始值是假的。可以回头看下第二章的例子：
```swift
var arrrow = CGRectZero
var body = CGRectZero
CGRectDivide(rect,&arrow,&body,Arrow.ARHEIGHT,.MinYEdge)
```
运行这段代码后，我们的的两个*CGRectZero*值会被替换；他们仅仅是一个占位，为了满足编译器的需求。

极少的情况下，你会调用一个立即返回值的Cocoa函数，然后用在后面的函数中会传入这个返回值。比如，Cocoa有个*UIApplication*的函数是这样声明的：
```swift
func beginBackgroundTaskWithExpirationHandler(handler:(() -> (void)?) -> UIBackgroundTaskIdentifier
```
这个函数返回一个*number*（UIBackgroundTaskIdentifier是一个Int类型），并且在后面会调用这个函数传递进去这个值（handler）--是一个返回的函数，这个函数中你会使用这个值。Swift的安全规则不会让你定义一个变量来保存这个值，然后再在一个匿名函数中使用这个值：
```swift
let bti = UIApplication.sharedApplication().beginBackgroundTaskWithExpirationHandler({
  UIApplication.sharedApplication().endBackgroundTask(bti)
}) //error：在初始化的时候必须使用*var*
```
因此，你需要使用*var*来声明；但是Swift有另一个报错：
```swift
  var bti : UIBackgroundTaskIdentifiler   
bti = UIApplication.sharedApplication().beginBackgroundTaskWithExpirationHandler({
  UIApplication.sharedApplication().endBackgroundTask(bti)
}) //error：变量在使用前必须初始化*var*
```
解决办法就是在声明这个变量的时候，给它一个初始值，作为占位值；

```swift
  var bti : UIBackgroundTaskIdentifiler = 0 
bti = UIApplication.sharedApplication().beginBackgroundTaskWithExpirationHandler({
  UIApplication.sharedApplication().endBackgroundTask(bti)
})
```

> **注意：**
> 一个对象的实例属性可以在对象的初始化方法中进行初始化，而不仅仅是在声明的时候。对于*let*和*var*声明的实例属性显示的表明类型而不直接初始化是很常见的。这个会在第四章讨论。



















