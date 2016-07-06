# 1.14 隐私

##隐私（privacy）

在之前，我讲过命名域不能足够安全的保证外边获取不到命名域内部的变量。但如果你喜欢它可以作为一个屏障。比如，不是所有实例的变量打算被其他对象获取。也不是实例的所有方法都可以被其他对象调用。任何正规的面向对象语言都需要一个隐私机制保护他们的成员--一种一个对象没有获的其他对象的权限下很难看到它的成员。

考虑下面的例子：

```swift
class Dog {
  var name = ""
  var whatADogSays = "woof"
  func bark(){
    print(self.whatADogSays)
  }
  
  func sepak(){
    print(self.whatADogSays)
  }
}
```

在这里，其他对象可以获取和修改属性*whatADogSays*。因为这个属性在方法**bark**和**speak**中使用，我可以很容易的让一个**Dog**实例说*meow*。比如下面：

```swift
let dog1 = Dog()
dog1.whatADogSays = "meow"
dog1.bark() //meow
```

你可能会说：为什么不使用**let**来声*whatADogSays*,让它成为一个常量！那其他对象就不可以改变它的值：

```swift
class Dog {
  var name = ""
  let whatADogSays = "woof"
  func bark(){
    print(self.whatADogSays)
  }
  
  func sepak(){
    print(self.whatADogSays)
  }
}
```
听起来是个不错的答案，但是还不够完美。这里有两个问题。如果我希望一个**Dog**实例自己能够改变变量的值怎么办。那么**whatADogSays**还是需要使用**var**声明；否则连变量自己都无法改变它的值了。或者我不希望其他对象知道实例says，但是我可以通过**bark**或者**speak**得到。虽然我们使用了**let**声明了变量**whatADogSays**但是其他对象仍然可以得到**whatADogSays**的值。这不是我想要的。

为了解决这个问题，Swift提供了关键字**private**。稍后我们会讨论这个关键字的结果，现在你只需要知道可以解决这个问题：

```swift
class Dog {
  var name = ""
  private var whatADogSays = "woof"
  func bark(){
    print(self.whatADogSays)
  }
  
  func sepak(){
    print(self.whatADogSays)
  }
}
```
现在**name**是一个公开变量，但是**whatADogSays**是一个私有变量：它不可以被其他变量获取。一个**Dog**实例可以调用**speak**获取**self.whatADogSays**，但是dog实例的引用就不可以，比如dog1,不可以使用*dog1.whatADogSays*。

*到这里本章的重点是：对象的属性默认是公开的，如果你希望将它变为私有的，你可以设置。类定义的时候就定义了命名域；这个命名域需要其他对象使用对外点语法获取命名域内部的成员，但是其他对象仍然可以获取到内部的命名域；在命名域内部，并没有设置一个是否可获取的一个门槛。关键字**private**提供了这样一个门槛*

