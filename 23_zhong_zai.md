# 2.3 重载

## 方法重载
*是指在同一个作用域中有两个或多个函数拥有相同的名字不同的签名（类型）*
在Swift中，函数重载是合理的（也是普遍的）。这意味两个函数只要具有不同的函数类型即使同名（包括外部参数名相同）也可以共存。

因此，下面的两个函数可以共存：
```swift
func say(what:String){
}
func say(what:Int){
}
```
重写起作用的原因是Swift具有严格的类型检测。*String*不是*Int*。Swift可以在声明的时候区分他们，同时Swift可以在函数调用的时候区分他们。因此，Swift可以区分*say("what")*和*say(1)*。

重载对于返回类型同样起作用。两个具有相同名字和参数类型的函数可以具有不同的返回值类型。但是调用的上下文必须明确；就是，必须明确期望的返回值类型。
比如，下面两个函数可以共存：
```swift
func say() -> String {
  return "one"
}
func say() -> Int {
  return 1
}
```
但是你调用的时候不能使用下面的方法：
```Swift
let result = say() //编译错误
```
这个调用是具有歧义的，编译器已经提示了。你必须在返回值明确的环境中调用这个函数。比如，设想我们有一个函数没有重载，并且希望一个String类型的参数：
```swift
func giveMeAString(s:String){
  print("thanks")
}
```
然后这个调用*giveMeAString(say())*是合理的，因为只走String类型的才可以进入函数，相同的是我们这样调用：
```swift
let result = say() + "two"
```
只有String类型的可以相加，所以这个也是合理的。

如果你从OC转过来的，你会对Swift中方法重载感到惊奇，在OC中重载是不允许的。如果你在OC中声明了两个重载版本的相同的函数，编译器会提示你声明重复的编译错误。同样的，如果你在Swift中声明了两个重载的方法，但是在OC中可以使用他们，你也会被编译器告知错误，因为这样的重载在OC中不允许。


> **要点：**
> 两个函数具有相同的函数类型（函数标志）但是不同的外部名称不属于重载；如果外部参数名称不同，这是两个不同的函数名字。


