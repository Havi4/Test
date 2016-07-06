# 3.4 计算变量（computed variables）

# 计算变量（computed variables）
到目前为止，本章讨论的变量都是存储型变量。提供了一个像盒子一样的。变量的名字就像一个盒子；一个值可以放到这个盒子，通过赋值的方式，然后这个值就会保存在那里知道被重新定义使用，只要这个变量还存在。

当然，一个变量可以是计算型的。这意味着这个变量不是有一个值，而是函数。当变量被赋值时调用的函数称为*setter*.当函数被引用的时候调用的函数称为*getter*。下面的代码说明了计算变量的语法：
```swift
var now: String {
  get {
    return NSDate().description
  }
  
  set {
    print(newValue)
  }
}
```
1. 变量声明的关键字只能是*var*.变量的类型必须显示的被声明。类型后面紧跟着花括号。
2. 获取的函数称为*get*。注意这里并没有声明一个正式的函数；*get*之后紧跟着函数体。
3. *getter*函数必须返回同类型的一个值。
4. 设置的函数称为*set*。同样也没有一个正式的函数声明；*set*后面紧跟着函数体。
5. *setter*方法就像一个带有一个参数的函数。默认情况下，这个参数在*setter*函数体内的局部名称是*newVlaue*。

下面是一些代码说明计算属性的使用。你就正常的看待这个参数，为了赋值给这个变量，给它赋值，需要使用使用的时候，直接使用。在这个背后，是*setter*和*getter*函数被调用：
```swift
now = "howdy"//howdy
print(now)
```
1. 给*now*赋值的时候会调用它的*setter*函数。这里传递给它的*setter*函数的参数是*howdy*。这个值在*set*函数中的局部名字是*newValue*。我们的*set*函数是打印*newValue*到控制台。
2. 获取*now*的值调用它的*getter*。我们的*getter*函数获取当前的时间然后转化为字符串，返回它。然后打印这个值。

注意我们给*now*设置*howdy*时候，*howdy*没有存在任何地方。比如，这是不起作用的。一个*set*函数可以存储一个值，但是它不可以存储给这个计算变量；计算变量不可以存储值！这个只是调用这个变量的*getter*和*setter*的缩写。

下面是几种基本语法的变种：
* *set*函数的参数中不是一定要定义为*newValue*。我们可以定义其他名字，把自己定义的名字放到*set*后面的大括号内：

  ```swift
  set (val) {
    //定义自己的函数体
  }
  ```
* 不一定会定义*setter*函数。如果*setter*函数省略，那么就变成了一个只读变量。尝试着给这个变量赋值会编译错误。在Swift中没有*setter*的计算变量是定义只读变量的主要途径。
* 但是计算变量必须有*getter*函数。但是，如果*setter*函数省略的话，关键字*get*和大括号都可以省略。因此，只读变量的定义是：
  ```swift
  var now: String {
    return NSDate().dexcrition
  }
  ```

计算变量有很多有用的，下面是在我编程中经常出现的：

* 只读变量
  
  计算变量是声明只读变量最简单的途径。只需要在声明中省略*setter*就可以。一般来说，只读变量都是全局变量或者属性；只读属性在局部变量没有意义。

* 函数的外壳

  每一次这个变量需要使用的时候，都可以通过计算函数，他经常可以作为表达只读计算变量的一个简单的语法，下面是例子：
  ```swift
  var mp : MPMusicPlayerController {
    return MPMusicPlayerController.systemMusicPlayer()
  }
  ```
  每次我想使用这个变量的是去调用*MPMusicPlayerController*并不麻烦，但是通过一个简单的名字*mp*可以更加的紧凑。既然*mp*代表同一个事情，而不是一个操作，把*mp*作为一个变量更加的美好，所以它所有的表现都是作为一个变量而不是函数。
* 其他变量的外壳

  计算变量可以在其他变量之前设备，作为这些存储型变量的一个看门者，决定这些变量如何设置和获取。这和OC的获取器很像。在一个例子中，在一个私有存储属性后面有一个公有计算属性：
  ```swift
  private var _p : String = ""
  var p :String {
    get {
      return self._p
    }
    
    set {
      self._p = newValue
    }
  }
  ```
  这是一个愚蠢的例子，因为我们并没有对我们的属性获取起做任何事：我们仅仅是直接的给这个私有变量设置和获取方式，所以在*p*和*_p*之间没有差别。但是根据模板，你可以在*setting*和*getting*时设置有意义的事情。
  

> **技巧：**
> 像上面的例子一样，一个计算变量可以引用其他实例属性；他也可以调用实例方法。这非常重要，因为一般来说初始化器不能做这两个事。这是因为，计算属性的函数只有在实例被初始化之后被调用的。

下面是一个计算变量作为存储实例的一个例子。我的类已经有一个实例属性来保持大量的存储属性值，这些可以被赋值nil：
```swift
var myBigDataReal: NSData! = nil
```

当我的app进入后台，我想释放我的内存。所以我打算把*myBigDataReal*作为一个文件存到本地，然后把变量赋值为nil，这样就释放了它的内存。现在考虑下，当我的app从后台进入前台，我的代码抓取数据的时候。如果它不为空，我获取它的值。如果为空，这可能是我们需要从硬盘读取。所以我们希望重新存储它的值。这可以使用计算属性完美实现。

```swift
var myBigData: NSData! {
  set (newData) {
    self.myBigDataReal = newdata
  }
  
  get {
    if myBigDataReal = nil {
      //从磁盘获取
      self.myBigDataReal = NSData(contentOfFile:f)
    }
    return self.myBigDataReal
  }
}
```


  



















