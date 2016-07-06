# 2.9 递归

## 递归

一个函数可以调用自己，这称为递归。递归听起来有点吓人，像是跳过悬崖，因为很容易产生一个无限循环。但是如果你可以正确的写一个函数，你会有一个停止条件来防止无线循环：

```swift
func countDownFrom(ix:Int) {
  print(ix){
    if ix > 0 {
      countDownFrom(ix -1)
    }
  }
}
```

