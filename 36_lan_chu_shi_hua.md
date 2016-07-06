# 3.6 懒初始化

# 懒初始化
*lazy*不是一个有偏见的评价；他只是一个重要行为的一般描述。如果一个存储型变量赋值一个初始值作为声明的一部分，并且使用了*lazy*初始化方法，那么它的初始值不会被计算，直到我们第一次获取这个变量的值。

下面是几种使用*lazy*进行初始化：
* 全局变量

  全局变量是自动懒加载的。这种你问你自己什么时候初始化变量是非常有意义的。当app启动的时候，首先遇到文件和他们顶层的代码。这个时候初始化全局变量没有意义，因为app还没有启动。因此全局变量初始化必须在某个时刻才有意义。因此，全局变量的初始化知道有其他代码引用它的时候才会初始化这个全局变量。在这之下，是由这个*dispatch_once*保证被执行一次；这可以保证初始化过程只进行一次，且是线程安全的。
  
* 静态属性（使用前缀static/class）

  静态属性的表现和全局变量很像，而且是差不多的原因。（因为Swift中没有存储属性，因此类属性不能初始化，也不能使用懒加载）
  
* 实例属性

  实例属性默认是不使用*lazy*加载的，但是可以通过在变量前添加*lazy*实现懒加载。这个属性必须使用*var*声明。这种类型的初始化器可能永远不会计算，这种情况是如果给这个属性赋值的代码在获取这个属性的代码前面。

*lazy*加载经常用来实现单例。单例是所有的代码都获取一个类的同一个实例：
```swift
class MyClass {
  static let sharedMyClassSinleton = MyClass()
}
```
现在其他的代码可以使用*MyClass.sharedMyClassSinleton*来获取*MyClass*的单例。单例会在自身第一次被引用的时候初始化；以后，其他代码不管引用多少次，实例返回的都是同一个对象。（观察上面，如果上面使用计算只读属性，返回这个单例会发生什么？为什么）

现在我们来讨论下实例属性的懒初始化。为什么有这个需求？有一个很明显的原因：初始化可能会产生恒大的代价，所以你需要避免一开始就避免生成，直到你需要的时候。但是有另外一个原因，一开始可能你没注意到但是是一个更加重要的：*lazy*加载可以做其他初始化器做不到的。实际上，它可以引用实例。一个正常的初始化器没办法做到，因为在运行初始化器的时候实例还没有被创建。相反的，一个*lazy*初始化器会在实例初始化完成之后进行，所以可以应用实例自己。比如，下面的代码如果没有添加*lazy*的话是不合理的：
```swift
class MyView : UIView {
  lazy var arrow :UIImage = self.arrowImage()
  func arrowImage () -> UIImage {
    //创建代码
  }
}
```
初始化一个懒加载实例的常用方法是使用匿名函数：
```swift
lazy var prog : UIProgressView = {
  let p = UIProgressView(progressViewStyle:.Default)
  p.alpha = 0.5
  p.trackTintColor = UIColor.clearColor()
  p.progressTintColor = UIColor.blackColor()
  p.frame = CGRectMake(0,0,self.view.bounds.size.width,20)//这里用了self
  p.progress = 1.0
  return p
}()
```
在Swift中有一些语言漏洞：懒加载实例不能设置属性观察，并且*let*也没有懒加载（所以没有办法让一个懒加载的实例是只读的）。但是这些限制不是致命的，因为你可以通过计算属性来实现懒加载实现不了的。

例子3-1手动实现懒加载属性
```swift
private var lazyOncer: dispatch_once_t = 0
private var lazyBacker : Int = 0

var lazyFont: Int {
  get {
    dispatch_once(&self.lazyOncer) {
      self.lazyBacker = 43
    }
    return self.lazyBacker
  }
  
  set {
    dispatch_once(&self.lazyOncer){}
    //willset
    self.lazyBacker = newValue
    //didset
  }
}
```
在例子3-1中，只有*lazyFont*是可以公开获取的；*lazybacker*是一个包装，*lazyOncer*保证只被初始化一次。因为*lazyFont*现在是计算变量，我可以观察它的*set*情况（把额外的观察函数放到set中。或者我们可以删掉*setter*变成只读的）。



















