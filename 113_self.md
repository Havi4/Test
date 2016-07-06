# 1.13 self

##self

一个实例就是一个对象，一个对象就是消息的接收者。因此，一个对象需要有一个给自己发送消息的方法。这就是关键字**self**的奇妙之处。这个关键字可以用在所希望使用的地方。

比如，我想保存**Dog**实例**bark**的内容--woof--到一个属性中。那么在我的**bark**实现中我需要引用那个实例。我可以这么做：
```swift
class Dog {
  var name = ""
  var whatADogSays = "woof"
  func bark(){
    print(self.whatADogSays)
  }
}
```
同样的，我希望添加一个实例方法**speak**和**bark**相近。在**speak**方法的实现中调用我本身的**bark**方法。代码：

```swift
class Dog {
  var name = ""
  var whatADogSays = "woof"
  func bark(){
    print(self.whatADogSays)
  }
  func speak() {
    self.bark()
  }
}
```

观察上面的例子中的**self**只出现在了实例方法中。当一个实例的代码中使用**self**指的是这个实例变量。如果表达式*self.name*出现在了**Dog**实例方法中，*self*指的是这个**Dog**实例，此时它的代码正在运行。

事实证明是，刚刚出现的**self**（我刚说明过的）都是可选的。你可以省略它，但是可以达到同样的效果：

```swift
class Dog {
  var name = ""
  var whatADogSays = "woof"
  func bark(){
    print(whatADogSays)
  }
  func speak() {
    bark()
  }
}
```
原因是如果你省略消息的接收者，那么你发出的这个消息就会被默认发送给**self**，编译器在底层会补全**self**作为消息的接收者。但是，我不会这么做（除非是误操作）。一个重要的原因是风格，我喜欢显式的使用**self**。我发现省略**self**的代码很难阅读和理解。还有一些地方我必须使用**self**，所以我跟倾向于显式的使用**self**。


