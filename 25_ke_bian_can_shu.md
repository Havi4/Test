# 2.5 可变参数
## 可变参数：可以接受任意多个同类型的参数

参数可以是一个变量。这意味着调用者可以提供足够多这个类型的参数，以冒号分割，函数体会把参数转化为数组。
为了说明这个参数是参数列表，后面跟着三个句号，像这样：
```swift
func sayStrings(arrayOfString:String...){
  for s in arrayOfStrings {
    print(s)
  }
}
```
调用的时候：
```swift
sayStrings("hey", "bo", "money")
```
在Swift早期的版本中，可变参数必须是最后一个参数；在是在版本2.0中取消了这个限制。现在的限制是一个函数最多只能声明一个可变参数（因为否则没办法决定可变参数的结尾）。比如：
```Swift
func sayStrings(arrayOfString:String...,times:Int){
  for _ in 1...times {
    for s in arrayOfStrings {
      print(s)
    }
  }
}
```
调用的时候：
```swift
sayStrings("hey", "bo", "money", times:3)
```
全局函数*print*带有可变参数，所以你可以在一行中打印出多个值：
```swift
print("manny",3,true) // manny 3 true
```
默认参数提示了输出值的格式。默认值*separator:*是一个空格（在你提供多个参数的时候），默认值*terminator:*是代表新的的一行：
```swift
print("mannie", "moe", separator:",", terminator: ",")
print("jack")
```

> **警告：**
> 不幸的是，在Swift中有一个漏洞：就是没有办法将数组转换为以逗号分割的列表，如果你以一个成员为某个类型的数组开始，你不能把它用到可变参数。


