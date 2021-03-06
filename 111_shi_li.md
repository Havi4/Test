# 1.11 实例

##实例

**类、结构体、枚举**这样的对象类型都有一个共同的特征：他们都可以被初始化。实际上，当你声明一个对象类型的时候，你只是定义了一个类型。初始化一个类型是在做一件事--就是进行初始化一个实例。

所以，比方说，我可以声明一个Dog类，并且我可以给我的类一个方法：
```swift
class Dog {
  func bark() {
    print("woof")
  }
}
```
但是实际上在我的程序中还没有任何一个**Dog**对象。我仅仅是描述了**Dog**这样的一个类是什么样子的。为了获得一个真正意义的**Dog**，我们还需要实例化一个。在实际创建一个**Dog**对象的过程就是初始化一个**Dog**实例。最后的结果是一个新的对象被创建。

在**Swift**中，创建一个实例可以将对象类型作为函数的名字然后调用这个函数。这就是涉及到了使用括号。当你在一个类型后面跟着一个括号的时候，你实际上是给这个对象类型发送了一个特殊的消息:初始化自己！

所以现在使用下面的初始化一个**Dog**实例：

```swift
let fido = Dog()
```
在上面的一行代码中还隐藏着好多的操作！我做了两件事：
1. 初始化了一个**Dog**对象，因此最后以**Dog**实例结束
2. 同时将**Dog**实例放到名为**fido**的这样的盒子里--我声明了一个变量然后使用**Dog**实例初始化了这个变量。

现在**fido**是一个**Dog**实例。（更重要的是，我使用了关键字**let**，**fido**永远是同一个**Dog**实例不会改变。我也可以使用关键字**var**,但是将**fido**初始化为一个**Dog**类型意味着之后**fido**只能是同一个类型）。

既然我拥有一个**Dog**实例，我可以给这个实例发送消息。你猜择校消息是什么？他们是**Dog**的属性和方法！比如：
```swift
let fido = Dog()
fido.bark()
```
上面的代码是合理的。不仅如此，还是起效果的：它实际上会在控制台输出"woof"。我实例化了一个dog并且可以叫！
![图1-1](屏幕快照 2016-05-10 18.45.20.png)

这里有一个重要的结论，所以我们来强调一下。默认情况下，属性和方法都是实例的属性和方法。你不可以使用这个给一个类型对象发送消息；你必须给一个实例发送消息。因此，下面的代码是不合法的且不会被编译：
```swift
Dog.bark() //编译错误
```
可以定义一个**bark**函数使得上面的**Dog.bark()**合法，但是这个是另一个类型的函数--类方法或者静态函数--你需要定义这样的函数。

属性同样具有这样的情况，为了证明，我们给**Dog**一个*name*属性。唯一的注意的是从现在开始每个实例初始化的时候都会有一个为**name**的属性。但是这个**name**不属于**Dog**对象内部的。**name**属性是：
```swift
class Dog {
  var name = ""
}
```
这就可以允许我给一个**Dog**的名字，但是这需要一个**Dog**实例：
```swift
let fido = Dog()
fido.name = "Fido"
```
同样也可以定义一个属性可以使用**Dog**调用**Dog.name**，但是那是另外一中属性--类属性或者一个静态属性--你到时候会用到的。

