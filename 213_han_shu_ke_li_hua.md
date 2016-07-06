# 2.13 函数柯里化

## 柯里化函数

再回头看下函数
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
我有点不喜欢这个函数：创建一个框的大小是一个参数，但是这个尺寸的圆角是硬编码。我希望可以根据自己来定义这个圆角。我有两个方法实现。其中一个是给这个函数一个参数：
```swift
func makeRoundRectangleMaker(sz:CGSize,_ r:CGFloat) -> () -> UIImage {
  return {
    imageOfSize(sz){
      let p = UIBesizerPath(roundedRect:CGRect(origin:CGPointZero,size:sz),cornerRadius:r)
      p.stroke()
    }
  }
}
```
我们调用的时候可以这样：
```swift
let maker = makeRoundRectangleMaker(CGSizeMake(45,20),8)
```
但是这里还有另外一个办法。我们从*makeRoundRectangleMaker*返回的参数没有参数。相反他可以带有额外的参数：
```swift
func makeRoundRectangleMaker(sz:CGSize) -> () -> UIImage {
  return {
  r in
    imageOfSize(sz){
      let p = UIBesizerPath(roundedRect:CGRect(origin:CGPointZero,size:sz),cornerRadius:r)
      p.stroke()
    }
  }
}
```
现在返回的函数带有一个参数，所以我们必须在调用的时候提供这个参数。
```swift
let maker = makeRoundRectangleMaker(CGSizeMake(45,20))
self.myImageView.image = maker(8)
```
如果我们不需要使用*maker*，我们可以将这些写在一行--调用一个函数后返回一个函数然后立刻生成我么需要的图片：
```swift
self.myImageView.image = makeRoundRectangleMaker(CGSizeMake(45,20))(8)
```
当一个函数返回一个带有参数的函数的时候，我们成这个函数是柯里化函数。你可以省略第一个箭头和最上层的匿名函数。
```swift
func makeRoundedRectangleMaker(sz:CGSize)(_ r:CGFloat) -> UIImage {
  return imageOfSize(sz){
    let p = UIBezierPath(
      roundRect:CGRect(origin:CGPointZero,size:sz),cornerRidus:r
    )
    p.stroke()
  }
}
```
表达式*（sz:CGSize）(_ r:CGFloat)*两个参数列表在一个箭头中，没有箭头区分--这意味着，"Swift,请给我柯里化这个函数"。这里Swift会给帮助我们把这个函数分成两个函数。其中一个函数*makeRoundedRectangleMaker*带有参数*CGSize*和另一个匿名函数带有参数*CGFloat*.我们的代码看起来是返回一个图片，但实际上是返回一个函数--这个函数返回一个图片。我们也可以像之前那样调用它。

