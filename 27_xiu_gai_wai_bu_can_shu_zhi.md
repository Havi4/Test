# 2.7 修改外部参数值

## 可以修改外部参数

在一个函数内部，一个参数本质上是一个局部变量。默认情况下，它是使用*let*声明的变量。你可以试着给变量赋值：
```swift
func say(s:String, times:Int, loudly:Bool){
  loudly = true //编译错误
}
```
如果你需要给函数体内的一个参数赋值，你需要显示的使用*var*声明这个参数：
```swift
func say(s:String, times:Int, var loudly:Bool){
  loudly = true 
}
```
在上面的代码中，参数*loudly*仍然是一个局部变量，改变这个参数的值不会改变函数体外没任何参数的值。但是，可以通过配置参数来改变函数体外的参数的值。一种类型是你希望你的函数返回不止一个结果。比如，我写一个函数来使用一个字符串替换给定的字符串的出现的所有的。返回出现的次数：
```swift
func removeFromString(var s:Sring, character c:Character) -> Int {
  var howMany = 0
  while let ix = s.characters.indexOf(c) {
    s.removeRange(ix...ix)
    howMany += 1
  }
  return howMany
}
```
在这里你可以这样调用
```swift
let s = "hello"
let result = removeFromString(s,character:Character("l")) // 2
```
这是非常好的，但是我们忘了一个重要的事情：原始的字符串，仍然是"hello",在函数体内，我们从参数的副本中移除了所有出现的字符，但是它并没有改变原始的字符串。

如果我们希望传到参数的原始值，我们必须做三个变化：
* 我们希望改变的参数必须使用关键字**inout**来声明。
* 在调用的时候，我们希望函数改变的值的必须使用**var**来声明，而不是**let**
* 不是将一个值作为参数传递，我们传递的是它的地址。调用这个是通过使用地址符**&**。

让我们来做一些改变。声明的时候：
```swift
func removeFromString(inout s:String, character c:Character) -> Int{
}
```
我们调用的时候：
```swift
var s = "hello"
let result = removeFromString(&s, character:Character("1"))
```
在函数调用后，**result**是2，字符串**s**是"heo",注意在函数调用中我们第一个参数前面的符号！我喜欢这个需求，因为它可以使我们明确的告诉编译器和我们，我们所做的事情是危险的，我们的函数是有副作用的，会改变函数外的值。

> **注意：**
> 当一个具有**input**参数的函数被调用的时候，变量的地址作为参数会始终传递个函数，即使函数没有对这个参数做任何变化。

当你使用**cocoa**的时候你经常会碰到这样模式的变量。*Cocoa*的API使用c或者OC写的，所以你很少看到Swift版本的*inout*。你可能会看到一些疑惑的类型比如**UnsafeMutablePointer**。从你作为调用者的焦距来看，和上面说描述的是一样的。你是定义了一个*var*变量，然后传递它的地址。

比如，考虑**Core Graphics**的函数**CGRectDivide**。*CGRect*是一个代表范围的结构体，当你希望将一个矩形分割为两个的时候，你可以调用这个函数。*CGRectDivide*需要你提供两个矩形结果。因此它是返回两个矩形，实际是，"你给我两个**CGRect**作为参数，我会给你修改他们，这个就是这个函数的结果"
下面是在Swift中的函数定义：
```swift
func CGRectDivide(rect: CGRect, _ slice :UnsafeMutablePointer<CGRect>, _ remainder: UnsafeMutablePointer<CGRect> , _ amout: CGFloat, _ edge: CGRectEdge)
```
第二个和第三个参数都是一个指向CGRect的指针。下面是我调用这个函数的代码，看下我第二和第三个参数：
```swift
var arrow = CGRectZero
var body = CGRectZero
CGRectDivide(rect, &arrow, &body, Arrow.ARHEIGHT, .MinYEdge)
```

在使用前，我必须创建两个CGRect变量，他们必须被初始化，然后会在函数中被替换，因此我使用*CGRectZero*作为默认值。

> **要点**
> Swift给*CGRect*提供了扩展，这个方法是Swift独有的，它返回两个值，因此，你可以避免调用这个函数，你也可以使用，但是你必须搞清楚原理。

有时候，*cocoa*使用使用你的函数的时候传递UnsafeMutablePointer参数，你希望改变它的值。为了达到目的，你不能直接给它赋值，就像我们在*removeFromString*中做的那样。你是在让OC函数不是Swift，并且这是一个UnsafeMutablePointer参数而不是inout参数，这里的拘束是赋值geiUnsafeMutablePointer的内存，下面是一个例子：
```swift
func popoverPresentationController(
  popoverPresentationController:UIPopoverPresentationController
  willRepositionPopoverToRect Rect: UnsafeMutablePointer<CGRect>, inView View:AutoreleasingUnsafeMutablePointer<UIView?>{
    view.memory = self.button2
    rect.memory = self.button2.bounds
  }
)
```
这是一个非常常见的不使用**inout**来改变参数值的情况--一般的，在这个参数是一个类的实例的情况。这个是类的一个特有的特征，和其他两个对象类型的区别。
String不是一个类，它是结构体。这就是我们为什么必须使用关键字**inout**来该改变String参数。所以我会声明一个**Dog**类来说明：
```swift
class Dog {
  var name = ""
}
```
下面是一个带有*Dog*实例参数和一个String参数的函数，并且给*Dog*的实例属性*name*赋值。注意到没有使用关键字*inout*：
```swift
func changeNameOfDog(d:Dog, to toString:String) {
  d.name = tostring
}
```
下面是使用这个函数：
```swift
let d = Dog()
d.name = "fido"
changeNameOfDog(d, to:"rover")
print(d.name) // "rover"
```
观察到我们改变了*Dog*实例属性，即使我们没有使用关键字*inout*，甚至是使用关键字*let*声明。这个貌似和我们定义的改变参数的规则违背--但是不是。这个是类的一个特征，因为他们自己就是可变的。在函数*changeNameOfDog*中，我们并不是在试着改变参数的自己。如果那样做，我们会拥有一个不同的*Dog*实例。这不是我们希望的，如果我们想要这么做，我们需要使用关键字*inout*声明参数。（并且**d**必须使用*var*来声明。）


> **注意：**
> 专业角度来说，类是一个引用类型，但是其他的是值类型。当你给一个函数传递一个结构体参数时，你实际上是赋值了一份结构体副本。但是当给一个函数传递一个类的实例的时候，


