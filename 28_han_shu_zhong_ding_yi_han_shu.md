# 2.8 函数中定义函数
## 函数中定义函数

一个函数可以在任何地方定义，甚至是一个函数体内。在一个函数体内定义的函数（局部函数）被相同作用的代码成为变量。但是它是不能被作用域外的获取到的。

函数的这个特征是非常好的，它的唯一目的是帮助其他函数，如果只有函数A需要函数B，那么函数B就可以定义在函数A的内部：
下面是我的App中的一个例子：
```swift
func checkPair(p1:Piece, and p2 :piece ) -> Path? {
  func addPathIfValid(midpt:Point, _ midpt:Point) {
  }
  for y in -1...yct {
    addPathIfValid((pt1.x,y),(pt2.x,y))
  }
  
  for x in -1...xct {
    addPathIfValid((x,pt1.y),(x,pt2.y))
  }
}
```
我在第一个循环和第二个循环中的做的是事情是一样的--但是具有不同的开始值。我们可以将可以将这个函数写到每个循环中，但是这样是没必要的，并且容易产生重复。（这样的重复成为"DRY"）。为了避免这样的重复，我可以重构重复的代码使他成为一个实例方法，让这个函数可以同时适用于两个循环，但是这样做又会产生一个问题，这个实例方法的可见范围太大，我们需要只有这两个循环可以调用这个函数，因此出现了局部函数。

有时候，一个函数只会在一个地方调用，这时候使用局部函数更加的合理。下面是另外一个例子：
```swift
func checkPair(p1:Piece, and p2:Piece) -> Path? {
  //...
  if arr.count > 0 {
    func distance(pt1:Point, _ pt2:Point){
      //计算两点之间的距离
      let deltax = pt1.0 - pth2.0
      let dletay = pt1.1 - pth2.1
      return sqrt(Double(deltax *deltax + deltay * deltay)
    }
    
    for thisPath in arr {
      var thisLength = 0.0
      for ix in 0..<(this.count - 1){
        thisLength += distance(thisPath[ix],thisPath[ix + 1],)
      }
    }
  }
}
```

再次，结构是清晰的（即使代码中使用了一些我还没有讨论的特征）。在函数*checkPair*中，当我有一个path的数组是，我需要知道每个路径的长度。每个路径本就是一个包含点的数组，所以需要知道它的长度，我需要计算每一组点的距离为了获得每一组点的距离，我使用了。我可以使用方法来计算循环中的值，因此，我使用分割的函数，距离和循环。

没有规定说代码要有多少行；事实上，声明*distance*是我的代码更长；严格来说，不是的，这个函数在我的代码中时出现一次，通常情况下，将这段代码抽象出来作为一个计算距离的工具可以是我的代码更加清晰：事实上，函数名字，距离，使我的函数更有意义；这是更容易理解和维护，比起将这个方法写在内部。

> **警告：**
> 局部函数也是局部变量，因此在意作用域中不能出现同名的函数，且不能和局部变量同名。


