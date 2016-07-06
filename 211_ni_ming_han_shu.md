# 2.11 匿名函数

## 匿名函数

考虑下之前的代码：

```swift
func whatToAnimate() {
  self.myButton.frame.origin.y += 20
}
func whatToDoLater(finsished:Bool) {
  print(finished)
}
UIView.animateWithDuration(0.4,animation:whatToAnimate,completion:whatToDoLater)
```
上面的代码有点难看。我声明了函数*whatToAnimate*和函数*whatToDoLater*，因为我希望在最后一行的将这两个函数作为参数传递给函数。我并不需要这个函数的名字做其他的操作；或者说这两个函数除了最后一行不会再被使用了。因此，如果可以仅仅传递函数体而不去声明这些函数的话是很好的选择。

这个就是匿名函数，在Swift中是常见的。为了声明匿名函数，你需要做两件事：
1. 创建函数体，包括包含函数体的花括号，但是不需要函数声明。
2. 如果有必要，在函数的第一行写出参数列表和返回值类型，然后后面写上关键字*in*。

下面我们来练习将我们的函数转化为匿名函数。注意我已经将参数和返回类型一草了花括号里面。
```swift
{
  () -> () in
  self.myButton.frame.origin.y += 20
}
```
下面函数*whatToDoLater*的声明：
```swift
func whatToDoLater(finsished:Bool) {
  print(finished)
}
```
下面是匿名函数的转变：
```swift
{
  (finished:Bool) -> () in
  print("finished")
}
```
既然我知道如何声明匿名函数，让我们使用他们。我使用匿名函数将他们作为参数传递。我们可以创建匿名函数然后传递进去：
```swift
UIView.animateWithDuration(0.4,animations:{
  ()->() in 
  self.myButton.frame.origin.y += 20
}, completion: {
  (finished:Bool)->() in
  print("finished")
})
```
现在我们知道，我们不需要单独的声明*drawing*函数。我可以给*imageOfSize*传递一个匿名函数：
```swift
let image = imageOfSize(CGSizeMake(45,20),{
  let p = UIBezierPath(roudedRect:CGRectMake(0,0,45,20),cornerRadius:8)
  p.stroke()
})
```
在Swift中匿名函数非常常见，所以你要确定你可以读懂和谐这样的代码。事实上，匿名函数非常常见和重要，下面是一些匿名函数的缩写：
1. 省略返回类型
> 如果匿名函数的返回类型是已知的，你可以省略箭头和返回类型的定义：
> ```swift
  > UIView.animateWithDuration(0.4,animations:{
  () in 
  self.myButton.frame.origin.y += 20
}, completion: {
  (finished:Bool)->() in
  print("finished")
})
> ```
2. 如果没有参数可以省略*in*关键字行
> 如果匿名函数没有参数，并且他们的返回类型可以省略，那么*in*所在的这行都可以省略：
>  
>  ```swift
>  UIView.animateWithDuration(0.4,animations:{
  self.myButton.frame.origin.y += 20
}, completion: {
  (finished:Bool) in
  print("finished")
})
> ```

3. 省略参数类型
> 如果匿名函数带有参数，并且他们的类型可以被编译器推导出，他们可以省略：
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  self.myButton.frame.origin.y += 20
}, completion: {
  (finished) in
  print("finished")
})
> ```
4. 省略括号
> 如果参数类型可以省略，参数类型外面的括号也可以省略：
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  self.myButton.frame.origin.y += 20
}, completion: {
  finished in
  print("finished")
})
> ```
5. 可以省略*in*行即使带有参数
> 如果返回类型省略，且参数类型可以由编译器推断，你可以省略*in*行，并且可以使用简单的符号$0,$1...来应用参数：
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  self.myButton.frame.origin.y += 20
}, completion: {
  print($0)
})
> ```

6. 省略参数名称
> 如果匿名函数不需要引用参数，你可以使用*_*来省略内部参数；事实上，匿名函数不需要引用任何参数，你可以省略所有的参数列表：
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  self.myButton.frame.origin.y += 20
}, completion: {
  _ in
  print($0)
})
> ```
> **小结：**
> > 记住，如果匿名函数带有参数，你必须声明他们。你可以省略*in*行使用缩写$0,$1..，你也保留*in*行，使用*_*忽略参数，但是你不能同时省略*in*行且不使用缩写。否则你的代码无法编译。

7. 省略函数参数标签
> 如果你的匿名函数是函数的最后一个参数，你可以把最后一个匿名函数提到函数的大括号外面，然后把匿名函数放到外面：（这称为尾部函数）：
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  >  self.myButton.frame.origin.y += 20
> }) {
  > _ in 
  > print("finished")
> }
> ```
8. 省略函数调用括号
> 如果你使用尾随函数语法， 如果你调用的函数没有参数，并且要作为参数传递，你可以省略空的括号。这个仅仅限于你省略调用时的括号。为了说明，我定义了下面两个函数：
> ```swift
> func doThis(f:()->()) {
  > f()
> }
> doThis{
  > print("woof")
> }
doThis(
  {
    ()->() in 
    print("howdy")
  }
)
> ```

9. 省略关键字*return*
> 如果匿名函数的函数体只包含一个语句，并且这个语句含有*return*关键字，这个关键字可以省略。换句话说，在一个上下文中，希望函数返回一个值，如果匿名函数体只有一条语句，Swift认为这个表达式的结果就是函数的返回的结果：
> ```swift
> func sayHowdy() -> String {
  >  return "howdy"
> }
> 
func performAndPrint(f:()-> String) {
  let s = f()
  print(s)
}
> 
performAndPrint {
  sayHowdy()
}
> ```

当写一个匿名函数的时候，你会经常法相你可以使用这些省略形式。此外，你经常会采用缩写形式通过把整个匿名函数和函数调用放到一行。因此，Swift代码包含匿名函数变的更加紧凑。

下面是典型的例子，我定义了一个Int类型的数组，然后让数组中的值乘以2后生成一个新的数组，主要是通过map方法。map方法接受一个带有一个参数的函数作为参数，返回一个值，这里，我们的数组是Int类型，所以我们需要传递一个map方法，这个方法带有一个Int参数并返回Int值。我们可以这样写：

```swift
let arr = [2,4,6,8]
func double(i:Int) -> Int {
  return i * 2
}
let arr2 = arr.map(double)//
```
但是，这个不是swifty。我不需要*double*做其他事情，且返回类型确定。这里只有一个参数，所以我们不需要*in*就可以获取参数。我们的函数体只有一条语句，因此可以省略return。map方法没有其他参数，所以我们可以省略括号，使用尾随函数：
```swift
let arr = [2,3,4,5]
let arr2 = arr.map{$0*2}
```


## 定义后立刻调用
在Swift中有一个很令人吃惊的模式，在一行中定义一个匿名函数然后立刻调用：
```swift
{
  //...代码
}（）
```
注意到花括号后面的大括号。花括号定义了匿名函数的函数体；大括号调用匿名函数。

为什么会有人这么做？如果你希望运行某段代码，你可以直接运行；为什么要把它嵌套到函数体内呢？然后运行函数体。

其中一个原因是，一个匿名函数可以让你的代码更加的函数化：一个操作可以在需要的时候进行，而不是开始就一步一步的进行。下面是*Cocoa*的一个例子：我们创建一个*NSMutableParagrphyStyle*然后把它作为函数*addAttribute:value:range:*中：
```swift
let para = NSMutableParagrphyStyle()
para.headIndent = 10
//...更多的代码
content.addAttribute(
  NSParagrphyStyleAttributeName,
  value:para, range:NSMakeRange(0,1)
)
```
我发现上面的代码很丑陋。我们除了把*para*作为值传递外不再需要它：所以在一个函数中配置这个更好，作为value：的参数，Swift中我们可以这么做。我更喜欢下面的方式：
```swift
content.addAttribute(
  NSParagrphyStyleAttributeName,
  value:{
    let para = NSMutableParagrphyStyle()
    para.headIndent = 10
    //...更多的代码
    return para
  }()
)
//这个是匿名函数，因为参数返回值都为空，且返回值可以推断，所以可以隐藏*in*行
```
我会在第三章中详细讲解。

