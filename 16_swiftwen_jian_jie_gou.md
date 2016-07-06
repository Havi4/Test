# 1.6 Swift文件结构
##Swift文件的结构

一个Swift程序可以包括一个或者多个Swift文件。在Swift中，一个文件是一个基本的单元，并且有许多关于Swift结构的规则，这些规则在其内部使用。（我的前提是假设我们不是在main函数中）。在Swift最顶层只有某些代码可以运行—下面列出：

###Module Import 语句

模块是一个比文件高一级的单元。一个模块可以包含多个文件，在Swift中，一个模块中，文件之间是可以相互看到的；但是一个模块是看不到另一个模块里面的内容，除非使用import将模块导入。比如，这就是你在程序中一个文件的第一行是import UIKit。

###变量声明

在一个文静顶层声明的变量是全局变量：它的生命周期是和程序一起的。

###函数声明

在Swift文件顶层声明的函数是全局函数：整个iOS程序都可以看到这个函数和使用这个函数，不需要给任何一个对象发送消息

###对象类型声明

声明类，结构体，或者枚举

下面的例子中，是一个合法的Swift文件（仅仅是为了说明这个文件的作用）一个import声明，一个变量声明，一个函数声明，一个类声明，一个结构体声明，一个枚举声明：
```swift
import UIKit
var one = 1
func changOne() {
}
class Manny {
}
struct Moe {
}
enum Jack {
}
```
这是一个愚蠢和没有意义的例子，但是请记住，我们的目标是探索文件的结构和部分语言，这个例子很好的展示了。

幸运的是，上面例子中的花括号内都可以声明变量，函数，和类型对象。确实，任何这种结构的花括号都可以包含这些声明，所以，如果关键字if后面带有花括号，他们也可以包含变量声明，函数声明或者对象类型声明的，下面的例子：
```swift
func silly(）{
	if true {
		class Cat {}
		var one = 1
		one = one + 1
	}
}
```
你可能注意到我并没有说文件顶层的代码可以执行。这是因为它实际是不能执行的！只有一个函数才可以包含可以执行的代码，它可以包含任何执行代码，在前面的例子中，one = one + 1，这是一个可执行的代码，并且也是合法的，因为他存在一个函数体内。但是如果one = one + 1存在文件的最顶层，它就无法执行；在Cat类中也没有办法直接执行。

例子1-1是一个Swift文件，示意图说明了可能的情况。（忽略枚举中的变量声明，枚举中的顶层声明我会在后面讲解）

例子1-1，一个合格Swift文件的示意图
```swift
import UIKit
var one = 1   //全局变量，任何地方都可以看到的
func changOne() {
	let two = 2
	func sayTwo() {
		print(two)
	}
	class Klass {}
	struct Struct {}
	enum Enum {}
	one = two
}   //定义了一个全局函数，任何地方可以看到
class Manny {
	let name = "manny"
	func sayName() {
		print(name)
		class Klass {}
		struct Struct {}
		enum Enum {}
	}
}
struct Moe {
	let name = "moe"
	func sayName() {
		print(name)
	}
	class Klass{}
	struct Struct {}
	enum Enum{}
}
enum Jack {
	var name: String {
		return "jack"
	}
	func sayName(){
		print(name)
	}
	class Klass {}
	struct Struct {}
	enum  Enum {}
}
```

很明显，我们可以重新按照我们喜欢的梳理下：我们可以声明一个类里面包含另一个类，在包含一个类……,但是没有必要这么做。


