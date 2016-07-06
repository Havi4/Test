# 2.12 闭包

## 闭包

Swift函数都是闭包。这就意味着函数可以捕获（保存）在这个函数体作用域内的变量。什么意思呢？让我们回头看下这个例子，花括号组成一个作用域，这个作用域可以获取声明在花括号里面的变量和函数：
```swift
class Dog {
  var whatThisDogsSays = "woof"
  func bark(){
    print(self.whatThisDogsSays)
  }
}
```
在上面的代码中，*bark*的函数引用了变量*whatThisDogsSays*。这个变量相对于*bark*是个外部变量，因为它声明在函数*bark*外面。因为它和*bark*在统一作用域，因为函数*bark*可以获取到这个变量，因此函数*bark*内部也可以，所以，可以获取*whatThisDogSays*变量。

目前看来很好；但是我们知道，函数可以作为变量传递。事实上，它可以从一个环境传递到另一个环境！那么这么做的时候，变量*whatThisDogSays*发生了什么？
```swift
func doThis(f: Void -> Void) {
  f()
}

let d = Dog()
d.whatThisDogSays = "hello"
let f = d.bark
doThis(f) //hello
```
运行这段代码，在打印台输出"hello".

可能你不会对结果感到吃惊。但是仔细想想。我们没有直接调用*bark*。我们实例了一个*Dog*对象，然后把它的*bark*函数作为值传递给函数*doThis*，那么现在看来*whatThisDogSay*是*Dog*的一个实例属性。在函数*doThis*内部没有*whatThisDogSay*。事实上，*doThis*内部也没有*Dog*实例！但是调用*f()*仍然起到了作用。函数*d.bark*在传递后仍然可以获取到变量*whatThisDogSays*，在*doThis*之外声明的，即使*whatThisDogSays*在一个没有*Dog*实例甚至任何实例属性*whatThisDogSays*的环境中。

貌似是，*bark*函数在传递的过程中，把的环境也传递过去--即使他们没有被调用直到被传递给其他环境.所以*捕获*的意思是函数作为值传递的时候，它会携带它内部引用到的外部变量。这就使得函数成为一个闭包。

你时刻都在受益于函数的闭包甚至你都没有意识到它的存在；看下前面的例子
> ```swift
> UIView.animateWithDuration(0.4,animations:{
  >  self.myButton.frame.origin.y += 20
> }) {
  > _ in 
  > print("finished")
> }
> ```

上面的代码可能不够明显，但是重新考虑第二行，匿名函数作为参数传递给*animations:*。你可能会说：真的？按照cocoa的套路，在动画开始的某个时刻执行这个匿名函数，*cocoa*会找到*mybutton*,*self*对象的一个属性，
cocoa可以这么做，因为函数是一个闭包。这个属性被捕获并且保存在匿名函数中，因此，当匿名函数调用时，*button*开始移动。

### 闭包是如何增强代码的

一旦你理解了函数及时闭包，你可以使用这个特性来提高你的代码。闭包可以使你的代码更加普通，更加有用。下面，在之前的创建一个*image*的函数重构下
```swift
func imageOfSize(size: CGSize, _ whatToDraw:()->()) -> UIImage {
  UIGraphicBeginImageContextWithOptions(size, false, 0)
  whatToDraw()
  let result = UIGraphicGetImageFromCurrentImageContext()
  UIGraphicsEndIamgeContext()
  return result
}
```
下面我使用尾随函数调用这个函数：
```swift
let image = imageOfSize(CGSizeMake(45,20),{
  let p = UIBezierPath(roudedRect:CGRectMake(0,0,45,20),cornerRadius:8)
  p.stroke()
})
```
但是上面的代码包含了一个讨厌的重复。这是一个根据指定大小来创建一个图片的函数。我们重复输入了大小；*45，20*两次。只是愚蠢的。让我们把这个尺寸抽离出来：
```swift
let sz = CGSizeMake(45,20
let image = imageOfSize(sz),{
  let p = UIBezierPath(roudedRect:sz,cornerRadius:8)
  p.stroke()
})
```
变量*sz*声明在了匿名函数上一次的作用域，在匿名函数内部可以看到这个变量。因此我们可以在匿名函数内部获取它，匿名函数是一个函数，因此是一个闭包。因此匿名函数可以捕获这个引用，并带到imageOfSize的调用中。当函数*whatToDraw*调用的时候，可以捕获变量*sz*。这没有问题的，即使在*imageOfSize*中没有变量*sz*。

现在让我们深入点，我们硬编码了大小。试想下，我们会创建不同大小的图片。我想把这个函数封装为一个函数，变量*sz*是一个变量：函数返回一个图片：
```swift
func makeRoundedRectangle(sz:CGSize) -> UIImage {
  let image = imageOfSize((sz),{
  let p = UIBezierPath(roudedRect:sz,cornerRadius:8)
  p.stroke()
  })
  return image
}
```
可以看到我们的代码仍然有效。这里，匿名函数引用的变量*sz*是来自函数*makeRoundedRectangle*，这是个外部参数并且在匿名函数作用域内。匿名函数是一个闭包，当他传递给*imageOfSize*的时候，这个匿名函数捕获了这个值。

我们的代码变得更加的紧凑。调用*makeRoundedRectangle*并提供一个*size*,就可以得到一个图片。因此，我可以将函数调用，获得图片和设置图片放到一行进行处理：
```swift
self.myImageView.image = makeRoundedRectangle(CGSizeMake(45,20))
```

### 函数返回函数

现在我再深入讨论！不是返回一个图片，我们的函数可以返回一个函数，这个函数定义一个特定的大小。如果你没有看到过一个函数作为返回值被返回,你现在要屏住呼吸。毕竟一个函数可以被用作值。我们已经在函数调用时将函数作为参数了；现在我们要将一个函数作为返回值：
```swift
func makeRoundedRectangle(sz:CGSize) -> （）-> UIImage {
  func f () -> UIImage {
    let image = imageOfSize((sz),{
    let p = UIBezierPath(roudedRect:sz,cornerRadius:8)
    p.stroke()
    })
    return image
  }
  return f
}
```
下面我们分析下代码：
1. 函数声明部分就很难。这个*makeRoundedRectangle*的类型是什么？是*（CGSize）-> () -> UIImage*。这个表达式具有两个箭头。为了理解它，记住箭头后面永远都是返回值。所以*makeRoundedRectangle*是一个带有参数*CGSize*返回值为*（）-> UIImage*的函数。好的，*（）-> UIImage*又是什么？我们已经知道这个了，它是一个没带参数，返回值为Image的一个函数。所以
*makeRoundedRectangle*是一个带有*CGSize*返回函数的一个函数--这个返回值本身就是一个函数，没有参数，返回一个图片。

2. 在*makeRoundedRectangle*函数内部，我们第一步是声明了一个函数--我们打算返回这个函数，这个函数没有参数，返回值为Image。这里我们这个函数名字为f。这个函数的作用很简单：调用imageOfSize，传递一个匿名函数绘制一个image，然后返回这个Image。
3. 最后返回我们定义的这个f。这样我们完成了我们的抽象：我们可以返回一个不带参数、返回值为Image的函数。

可能你还在好奇*makeRoundedRectangle*这个函数，好奇你是如何调用这个函数，以及得到什么？
```swift
let maker = makeRoundedRectangle(CGSizeMake(45,20))
```
*maker*变量是如何运行的？这个是个函数--不带参数，调用后会产生一个Image，你相信我码？我证明给你--通过调用这个函数：

```swift
let maker = makeRoundedRectangle(CGSizeMake(45,20))
self.imageView.image = maker()
```
既然你对一个函数作为返回感到很目瞪口呆，下面我再次将目光聚集到*makeRoundedRectangle*的实现然后分析下。记住，我不是为了给你说明一个函数可以产生另一个函数。我这么做是为了说明闭包！我们来考虑下是如何捕获上下文的：
```swift
func makeRoundedRectangle(sz:CGSize) -> （）-> UIImage {
  func f () -> UIImage {
    let image = imageOfSize((sz),{
    let p = UIBezierPath(roudedRect:sz,cornerRadius:8)
    p.stroke()
    })
    return image
  }
  return f
}
```
函数*f*没有带参数。f的函数体内有两个地方，使用到了变量*sz*。函数*f*可以获取到*sz*，从函数*makeRoundedRectangle*传递进来的参数，因为他在外部作用域中。函数*f*在函数*makeRoundedRectangle*被调用的时候就捕获了变量*sz*,并在*f*被返回赋值后仍然保持状态：
```swift
let maker = makeRoundedRectangle(CGSizeMake(45,20))
```
这就是为什么*maker*是一个函数可以创建并返回一个特定大小的图片，即使自己没有参数。我们已经图片的尺寸给了*maker*。

从另一个角度看，*makeRoundedRectangle*是创建*maker*这样类似函数的一个工厂，每个都可以产生一个特定大小的图片。这个说明了闭包的强大之处。

在离开*makeRoundedRectangle*之前，我希望把它重构的更加Swiftier。在函数*f*中，没有必要创建函数im并返回它；我们可以直接返回调用*imageOfSize*的结果：
```swift
func makeRoundRectangleMaker(sz:CGSize) -> () -> UIImage {
  func f()-> UIImage {
    return imageOfSize(sz){
      let p = UIBesizerPath(roundedRect:CGRect(origin:CGPointZero,size:sz),cornerRadius:8)
      p.stroke()
    }
  }
  return f
}
```
同样也不要声明*f*然后返回他；可以使用匿名函数直接返回：
```swift
func makeRoundRectangleMaker(sz:CGSize) -> () -> UIImage {
  return {
    return imageOfSize(sz){
      let p = UIBesizerPath(roundedRect:CGRect(origin:CGPointZero,size:sz),cornerRadius:8)
      p.stroke()
    }
  }
}
```
又因为匿名函数只有一条语句，返回*imageOfSize*的结果，因此可以省略return。：

 ```swift
func makeRoundRectangleMaker(sz:CGSize) -> () -> UIImage {
  return {
    imageOfSize(sz){
      let p = UIBesizerPath(roundedRect:CGRect(origin:CGPointZero,size:sz),cornerRadius:8)
      p.stroke()
    }
  }
}
```

### 闭包设置捕获值

闭包捕获上下文中变量的能力比我展示给你的要强大的多。如果一个闭包捕获了自身以外的一个引用，并且这个值可以设置，闭包可以修改这个变量。

比如，我定义了一个简单的函数，它可以接受一个带有*int*参数的函数作为参数，然后调用这个参数：
```swift
func pass100(f:(Int) -> ()){
  f(100)
}
```
现在看下花括号，猜想下面的代码运行结果：
```swift
var x = 0
print(x)
func setX(newX:Int){
  x = newX
}
pass100(setX)
print(x)
```
第一个打印的值为0.第二个为100.*pass100*函数改变了变量*x*的值！这是因为我传递给*pass100*的函数包含了对x的引用；不仅包含它，还保持了这个状态；不仅保持了这个状态，还设置了它的值。这个x就是我的那个x，因此，*pass100*可以像直接调用*setx*那样改变x的值。


### 闭包可以保持捕获的上下文

一个闭包一旦捕获了上下文，它就会保持这个上下文，即使没有其他发生。下面的例子--一个函数修改一个函数：
```swift
func countAdder(f:()->()) -> ()-> () {
  var ct = 0
  return {
    ct = ct + 1
    print("count is \(ct))
    f()
  }
}
```
函数*countAdder*接受一个函数作为参数并返回一个函数。返回的这个函数调用参数函数，增加变量的值并打印出来。猜想下以下结果：
```swift
func greet() {
  print("howdy")
}
let countedGreet = countAdder(greet)
countedGreet()
countedGreet()
countedGreet()
```
我们在这里做的就是写一个函数*greet*，打印howdy，然后将它作为参数传递给*counterAdder*。*countedAdder*返回一个新的函数，我们赋值给*countedGreet*。然后我们调用*countedGreet*三次。下面是输出结果：
```swift
count is 1
howdy
count is 2
howdy
count is 3
howdy
```
很明显，*countAdder*被传入一个函数--这个函数可以打印被调用了几次。现在问下自己：维持这个数目的变量到底在哪里？在*countAdder*中，它是本地变量。但是它并没有声明在*countAdder*返回的匿名函数中。这是很惊奇的。如果它是声明在匿名函数中，我们可以每次给变量*ct*设置0.我们不能计数了。相反的，*ct*被初始化一次，然后被匿名函数持有。因此，这个变量变成*countedGreet*上下文的一部分--所以当函数*countedGreet*每次调用的时候，可以计数。这就是闭包的强大。

上面的例子，可以维持上下文状态，也可以帮助我们来证明函数是应用类型。为了说明我的意图。我重新声明一个。两个不同的函数说明是两个不同的函数：
```swift
let countedGreet = countAdder(greet)
let countedGreet2 = countAdder(greet)
countedGreet()//1
countedGreet()//1
```
在*countedGreet*和*countGreet2*中两个函数单独的维持了他们的计数。但是通过简单的赋值产生的变量是同一个函数。
```swift
let countedGreet = countAdder(greet)
let countedGreet2 = countedGreet
countedGreet()//1
countedGreet()//2
```


> 注意函数是应用类型

