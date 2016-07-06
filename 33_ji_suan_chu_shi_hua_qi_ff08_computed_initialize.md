# 3.3 计算初始化器（computed initializer）

# 计算初始化器（computed initializer）
有时候，你可能需要运行一段代码来计算一个变量的初始值。表达这个需求的一个简单紧凑的办法就是使用匿名函数，然后立即调用。下面是一个例子：
```swift
let timed: Bool = {
  if val == 1{
    return true
  }else {
    return false
  }
}()
```
当你初始化实例属性的时候你也可以做同样的事情。在这个类中，我需要在后面多次用到一个图片。提前定义一个常量实例来保存这个图片是很有意义的。创建的意义是绘制它。这个需要几行代码才能完成。所以我声明这个属性然后通过调用一个匿名函数来初始化它，像这样：
```swift
class RootViewController: UITableViewCOntroller {
  let cellBackgroundIamge: UIImage = {
    //（）-> UIImage in 这行省略了
    return imageOfSize(CGSize(320,44)){
      //draw code
    }
  }
}
```
事实上，一个定义调用匿名函数有时候是给一个实例属性初始化的唯一合理方法。原因是，当你初始化一个实例属性的时候，你不能够调用实例方法，因为此时还没有实例--实例正在创建的过程中。