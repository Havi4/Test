# 2.6 参数省略

## 省略参数

如果一个参数的内部名称是用**_**表示它是省略的。调用的时候必须提供参数，但是在函数体内没有名字，也不能被使用。比如：
```swift
func say(s:String, times:Int, louly _:Bool){}
```
在函数体内*louly*参数没有命名，但是调用这个函数的时候必须提供这个值。
对于忽略的参数不需要提供外部化的参数名：
```swift
func say(s:String, times:Int, _:Bool){}
```
这个特征的目的是什么？这个不是为了满足编译器，因为编译不会去抱怨一个参数没有在函数体引用。我使用它主要是来给自己一个提示"这里有个参数，但是我没有使用它"

