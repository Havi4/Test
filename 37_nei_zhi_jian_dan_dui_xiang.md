# 3.7 内置简单# 3.7 内置简单you对象

## 内置简单对象

每一个变量都必须有一个类型。但是类型到底是什么？到目前为止，我已经提到了一些类型，比如*Int*、*String*,打没有细致的讲解。下面是一些Swift内置的一些简单的类型，包含一些实例方法、全局方法、和一些操作。

### Bool
Bool对象类型（是个结构体）有两个值，*true*和*false*。你可以使用关键字*true*和*false*来代表这两个值，很自然的你可以想到**bool**对象要么是*true*要么是*false*。
```swift
var selected: Bool = false
```
在上面的代码中，变量*selected*被初始化为*false*；接下来你可以给这个变量设置*true*或者*false*。因为它是简单的*yes*或者*no*的状态，一个bool值经常被作为*flag*。

Cocoa的方法经常会希望能够传入一个*bool*参数，然后返回一个*Bool*值。比如，当你的app启动的时候，Cocoa会在代码中像下面这样调用：
```swift
func application(application:UIApplication, didFinishLauchingWithOptions launchOptions:[NSObject: AnyObject]?) -> Bool
```

    你可以在这个函数中做你喜欢的事情；通常情况下，你不需要做其他事情。但是你必须返回一个*Bool*值！在现实生活中，*Bool*值经常是*true*的。一个最简单的实现：
```swift
func application(application:UIApplication, didFinishLauchingWithOptions launchOptions:[NSObject: AnyObject]?) -> Bool {
  return true
}
```
*Bool*值在条件语句中很有用，我会在第五章中讲解，当你使用*if something*的时候，*something*是一个条件语句。一个*Bool*值--是一个可以转化为*Bool*值。比如，当你使用*==*来比较两个值的时候，它们的结果是一个*Bool*--如果他们相等为*true*，不相等为*false*：
```swift
if meaningOfLift = 42 {
  //
}
```
(后面我会讨论*==*，当我们讨论类型可以比较的，比如*Int*和*String*)

当准备这个条件时，你有时会发现一个加强版的会将*Bool*值保存到一个变量中：
```swift
let comp = self.traitCollection.horizontalSizeClass == .Compact
if comp {
  //
}
```
观察上面的情况，我们直接使用*Bool*变量作为条件。这是愚蠢的--并且是错误的--如果使用*if comp == true*，因为*if comp*已经意味着*if comp 是真的*这里没有必要再显示的判断*Bool*值；表达式本身就已经存在这样一个检测。

因为*Bool*值可以作为一个条件，调用函数返回的*Bool*可以作为一个条件。下面是我程序中的一个例子。我定义了一个返回*bool*的函数，来说明用户选择的*card*是否包含正确的答案：
```swift
func evaluate(call:[CardCell]) -> Bool {
  //
}
```
因此我可以这样使用：
```swift
if self.evaluate(cellsToTest){//..}
```
不像其他计算机语言，Swift中没有其他的会被作为*Bool*来看待，比如，在C中，*boolen*实际是一个数值，*0*代表false，但是在Swift中，除了*false*没有其他的值是false的，也没有其他的值是*true*。

*Bool*的名字来自于英语：*Boolen*的代数可以进行逻辑运算。*Bool*值可以使用下面的运算符：
* ！
  取反操作符会改变*Bool*的值，使用的时候在*bool*变量前面。
  
* &&
  逻辑与，只有当操作符两边都是*true*的时候返回*true*;否则，返回false。如果第一个操作数为*flase*，第二个操作数则不会进行检验（这样避免了可能的副作用）
  
* ||
  逻辑或，操作符两侧只要有一个操作数为*true*,那么它的值就是*true*；否则结果为*false*。如果第一个为*true*，则第二个操作数不会检验。
  
如果逻辑操作比较复杂，你可以使用括号来使得逻辑运算更加的清晰和有序。


### Numbers
数字类型主要是*Int*和*Double*，这取决于你自己的决定，这些都是你可能用到的类型。其他存在的数字类型主要是来和C或者OC进行兼容，Swift必须正确的知道这些类型。


####Int

*Int*对象类型（结构体）可以表是*Int.min*和*Int.max*之间的整数。能表示的值的范围依赖于app安装的平台和架构，所以不要绝对的计算这个值；我测试的64位的是2^63-1到-2^63.

表示*Int*类型值的最简单的方法是使用数值字面量。没有小数点的数值字面量默认是*Int*类型。在内部可以使用*.*来分割这是合理的，这可以是一个较大的数值方便阅读。前面加0也是可以的，这个可以在你的代码中赋值。

你可以定义一个二进制，八进制，十六进制的数值。你只需要在数值前加0b,0o或者0x。


####Double

*Double*类型（结构体）可以代表一个有15位的（64位机器上）的浮点小数。

定义*Double*最简单的方法是通过数值字面量。默认情况下，任何一个带有小数点的数值都被作为浮点型数值。*Double*也可以使用*.*来使大的数据表达清楚。

*Double*不能以*.*开头，因此代表一个0到1的数值是必须一*0.*开头。（我强调这个是因为要和Ch和OC区分）

你可以使用科学计数法表达一个*Double*类型。*e*代表10，后面的数值代表10的多少次方。你可以省略小数点如果分数为0的前提下。比如：
```swift
3e2表示3*10^2
```
你可以定义十六进制的*Double*。你只需要开头加上*0x*，在这里你也可以使用指数表示；
```swift
0x10p2表示64 = 16 * 2^2
```
这里还有一个静态属性*Double.infinity*和一个实例属性*isZero*.


####类型转换（Coercion） 
*Coercion*是把一个类型的值转化为另一个类型的。Swift没有显示真正的显示转换，但是它有一些方法可以实现这个目的--实例化。显示的把一个*Int*类型转化为*Double*类型，可以在*Double*实例化的大括号中写入*Int*值。反之亦然；但是这个会截断小数点。
```swift
let i = 10
let x = Double(i)
print(x) // 10.0,double

let y = 4.8 
let j = Int(y)
print(j) //4 ,Int
```
当一个数值被赋值给一个变量或者作为函数的参数的时候，Swift会做隐式的类型转换。下面的代码是合理的:
```swift
let d :Double = 10
```
但是下面这个不可以，因为你给一个变量赋值一个不同的类型，编译器会报错：
```swift
let i = 10
let d: Double = i//i的类型是Int，d的类型是Double。
```
解决的办法就是进行强制转换：
```swift
let i = 10
let d: Double = Double(i)
```
当两个数组变量进行数学操作运算是这个规则同样适用。Swift只会操作文字转换。下面是Int类型转化为Double。
```swift
let x = 10/3.0
print(x)// 3.3333333
```
如果你希望使用操作符运算这些类型你必须将不同的类型显式的转化。比如：
```swift
let i = 10
let n = 3.0
let x = i/n //编译错处，因为类型不同
```
这些很明显的使得Swift成为一个严格类型的类型检测；但是这些构成了Swift区别于其他语言对待数值的操作。我提供的这个例子很好的解决了，但是如果数学运算符很复杂，事情就比较复杂了，还有就是其他数值类型的存在，这些数值类型需要适配Cocoa。


####其他数值类型

如果你没有编写过iOS程序--你只单纯的使用Swift编写--你可能只会用到*Int*和*Double*类型的数值类型。非常不幸的是，编写iOS程序，你需要使用Cocoa，这里面包含了其他的数字型类型，并且Swift可以匹配他们。因此除了*Int*，还有其他类型的*int*--*Int8*、*Int16*、*Int32*、*Int64*--还有无符号类型的Int。除了*Double*还有32位的*Double*。另外，在*Core Graphics*中还有*CGFloat*它的值可以是float，也可以是Double，这个是由架构决定的。

当你使用C API的时候你可能会遇到c 类型的数值。这些类型仅仅是类型别名，他们是给当前类型定义了个名字。比如，CDouble仅仅是Double的一个别名，CLong是一个Int，在cocoa框架中还有好多的类型别名，比如，NSTimeInterval是Double的一个别名。

下面就有问题了。我已经告诉你，不可以赋值，传递或者运算不同类型的数值变量；你必须把他们转化为同一个类型。但是现在你会被cocoa中好多的数值类型搞崩溃。Cocoa提供的数值类型要么是*int*要么是*Double*--你不必刻意记住这些，知道编译器打断你，接下来你必须修正你的错误，然后转换位同一类型。

下面是我app的一个典型的例子。我们有一个UIImage，我们需要用它的CGImage，现在我们需要把CGImage的大小显示出来：
```swift
let mars = UIImage(named:"mars")
let marsCG = mars.CGImage
let szCG = CGSizeMake(
  CGImageGetWidth(marsCG),
  CGImageGetHeight(marsCG)
)
```
现在的问题是*CGImageGetWidth*和*CGImageGetHeight*返回的是*Int*，但是CGSizeMake希望的参数是CGFloats。这在c或者oc中没有问题，oc中可以隐式的把前者转化为后者，但是在Swift中，你必须显式的转化：
```swift
var szCG = CGSizeMake(
  CGFloat(CGImageGetWidth(marsCG)),
  CGFloat(CGImageGetHeight(marsCG))
)
```
这里还有一个现实生活中的例子。在界面上的*slider*是一个UISlider。它的*minimumValue*和*maximumValue*都是*floats*,在这段代码中，s是一个slider，g是一个UIGestureRecognizer,我们试着使用手势来控制slider的滑块的位置：
```swift
let pt = g.locationInViews(s)
let percentage = pt.x / s.bounds.size.width
let delta = perentage * (s.maximuValue - s.mimumValue) //编译错误
```
这段代码通不过编译。pt是一个CGPoint，因此pt.x是CGFloat，幸运的是，s.bounds.size.width也是CGFloat，所以第二行可以编译通过，*percentage*现在是CGFloat。但是在第三行中，我们试着运算*s.maximuValue - s.mimumValue*他们是Floats，不是CGFloat，因此我们必须改为：
```swift
let delta = CGFloat(perentage) * (s.maximuValue - s.mimumValue)
```
好消息是，你可以从编译器获取更多的信息，Xcode可以帮助你快速的发现变量的类型，使你追溯你的问题所在


> **技巧：**
> 极少的情况下，你需要把一个*interger*类型赋值给另外一个类型，但是不知道另一个类型具体是什么，此时你可以使用Swift的动态转换。比如，如果i和j在之前声明为不同的整形类型，i = numberiCast(j)可以把j转换为i的类型。


####运算操作符

Swift的数学运算符和你想象的一样；他们和其他计算机语言一样和现实生活中的操作符类似：
* +
  加号操作符，给第一个操作数相加，然后返回结果
  
* -
  减号操作符。从第一个错作数减去第二个，然后返回结果。还有一个作用是作为前缀，他返回一个和自身相反的。
  
* *
  乘法。两个数相乘，然后返回结果。
* /
  除法，两个数相处，返回结果
  
  > **警告：**
  > 像C中一样，一个Int类型除以Int类型得到一个Int类型的值，
* %
  取余。将第一个操作数第二个操作数分割然后返回余值。如果第一个数是负数的话结果可以说可以是负数；如果第二个数是负数，会它认为是正数，浮点型的也可以取余。

整型可以作为二进制因此可以进行位运算：

* &
  位与。只有两个位上的值都为1的话，结果才为1.
* |
  位或。两个位上都位0，这个位上的结果才为0.
* ^
  异或。只有两个二进制位上不一样结果才是1.
* ~
  取反。是一个单目运算符，将每一个位取反。
* <<
  左移.双目运算，操作的每一位向左移动指定位数。
* >>
  右移，双目运算，操作的每一位向右移动指定位数。

整型的上溢和下溢--比如说，相加两个*Int*类型的值，导致大于Int.max--是个运行时错误（你的app会闪退）。在简单的例子中，编译器会告诉你，
```swift
let i = Int.max -2 
let j = i + 12/2 //溢出
```
某些条件下，你可能希望强制这个操作成功，所以定义了特殊的溢出函数。下面的方法返回一个元组；我会在下面的类型讨论元组：
```swift
let i= Int.max - 2
let (j, over) = Int.addWithOverflow(i,12/2)
```
现在j是Int.min + 3(因为这个值已经从*Int.max*转化为*Int.min*)

如果你不关心是不是存在溢出，特殊的数学操作符可以使你避免错误：&+，&-，&*。

你可能像要把一个已知的变量和另一个变量操作，然后把结果赋值给原来的变量。你那样做可能需要使用*var*定义变量：
```swift
var i = 1
i = i + 7
```
作为一个简写，操作符提供了一个简写版本：
```swift
var i = 1
i += 1
```
可以简写的操作符有：+=，-=，*=，/=,%=,&=,|=,^=,~=,<<=,>>=。

有时候你会对一个变量进行自加或者自减。所以有一个自加自减操作符*++*和*--*，这个是前缀使用，此时这个变量先进行自加或者自减，然后在下面使用的时候是自加或者自减之后的值。如果是放在后面，那么在这个值被使用后才会进行自加或者自减。很显然，这个变量应该声明为*var*。

操作符具有优先级，比如\*的优先级高于+，所以*x + y * z*中y于z先进行，然后是加号，可以使用括号来划分优先级；
全局的函数包括abs,max,min:
```swift
let i = -7
let j = 6
print(abs(i)) //7
print(max(i,j)) // 6
```

其他的数学函数，比如开平方，取整，随机数，等等，都是C函数的，因为你导入了UIKit所以可以看到。你仍然需要注意数值类型，因为没有隐式转换，甚至是文字。

比如，*sqrt*希望参数是一个CDouble，所以你就不可以使用*sqrt(2)*，你只能说*sqrt(2.0)*。相似的，*arc4random*返回一个UInt32.如果你希望获取一个0到n-1的整数，你不能使用*arc4random()%n*,你需要把结果转化为一个Int类型。


####比较
数值可以使用比较操作符进行比较，返回的结果是*bool*。比如：表达式*i==j*比较i和j是不是相等；当i和j是numbers时，相等意味着值相等。所以*i==j*在i和j具有相等的值时是相等的，下面是比较操作符：
* ==
  等于操作符
* ！=
  不等操作符
* <
  小于。如果第一个值小于第二个值返回true。
* <=
  小于等于。
* >
  大于
* \>=
  大于等于
时刻记住，由于计算机的存储性质，Double类型的值进行相等比较的时候可能会失败。比较两个*double*值是不是相等的，可以通过比较两个值的差值和一个很小的值的比较：
```swift
let isEqual = abs(x - y) < 0.0000001
```

### String(字符串)
*String*(结构体)表示文本内容。表示字符串最方便的方式是字面量，直接使用双引号：
let greeting = "hello"
Swift的字符串是彻底的现代化的语言；从底层开始，你可以直接在字符串中包含任何字符，如果你不希望输入Unicode,可以使用术语*\u{...}*,大括号里面是八进制字符：
```swift
let leftTripleArrow = "\u{21DA}"
```
字面量中的反斜杠是一个转义字符；它意味着，"我不真的是一个反斜杠,我表示后面紧跟的字符是有特殊意义"。大量的不可打印的字节具有特殊意义：
* \n
  换行
* \t
  tab字符
* \"
  一个引号
* \\
  反斜杠
Swift比较酷的一个特征就是插入。这可以让你在输出语句中插入任意的变量，及时自身不是一个字符串。语言是*\(...)*:
```swift
let n = 5
let s = "You have \(n) widgets"
```
现在字符串是*You have 5 widgets*,但是这个例子不是让人信服的，因为我们知道5是什么类型，并且可以直接的插入到字符串中；但是想想如果我们不知道n的类型！更重要的是，在括号里面的东西不一定是一个变量的名字；它可能是任意一个合理的Swift表达式。如果你不知道如何添加，下面是更多的例子：
```swift
let m = 5
let n = 4
let s = "you had \(m + n)widges"
```
其中一个不允许的是在在大括号里面有双引号。这有点让人失望，但是这个不是什么困难；只要赋值给一个变量，然后使用这个变量即可。比如，下面的写法是错误的：
```swift
let ud = NSUserDefaults.standardUserDefaults()
let s = "you \(ud.integerForKey("widgets"))"
```
省略大括号并不能起到作用。你必须分成多行，像下面这样：
```swift
let ud = NSUserDefaults.standardUserDefaults()
let n = ud.integerForKey("widgets")
let s = "you \(n)"
```
为了合并两个字符串，最简单的方式是使用*+*.:
```swift
let s = "hello"
let s1 = "word"
s.appendContentsOf(s2) // s += s2
```
另外一个拼接字符串的方式是*joinWithSeparator*方法。你必须使用一个数组开头然后在他们之间插入需要插入的字符：
```swift
let s = "hello"
let s2 = "word"
let space = " "
let greeting = [s1,s2].joinWithSeparator(space)
```
这个快捷的表示方式可以进行是因为*+*可以进行重载：如果两个*String*类型的值相等，指的是具有相同的字符串内容。

还有一些便利的实例和类方法。*isEmpty*返回一个*bool*值来判断一个string是不是空的。*.hasPrefix*和*hasSuffix*判断一个字符串是不是以某个字符串开头或者结尾；比如，*hello*是一个*he*开头，*uppercaseString*和*lowercaseString*提供转换大小写的功能。

在*int*和*string*之间进行强制转化是可以的。为了使一个字符串代表*int*,使用*string*的转换方法是非常有效的；相反的，将*int*作为*string*初始化方法中的参数，就像你在number之间进行的转化一样：

```swift
let i = 31
let s = String(i,radix:16) // "1f"
```

代表数字的一个字符类型可以转换位数值类型；一个整型类型可以接受一个浮点类型。转换可能失败，因为字符串可能不代表特定类型的一个数字；所以不是一个数值，而是一个可选包装的数值类型（目前还没有讨论可选类型），所以现在你先相信我：

```swift
let s = "31"
let i = Int(s) // Optional(31)
let s2 = "1f"
let i2 = Int(s2, radix:16)// optional(31)
```

> *注意：*
> *String*的强制转换实际上是*string*的一个扩展，你可以使用*console*来显示。你可以是任何对象转换位String，通过实现三个协议：*Streamable*,*CustomStringConvertible*,*CustomDebugStringConvertible*.在下面介绍协议的时候我会举个例子：

```swift
let s = "hello"
let length = s.characters.count //5
```
为什么这里*string*有一个简单的*length*属性？这是因为，一个*String*并不真的有一个*length*。*String*是一系列*unicode*的存储，但是可变的*unicode*是可以合并的；所以，为了知道有多少个字节被这样的序列代表，我们实际上遍历了这个序列，然后以它代表的字节来处理。

所以你可以遍历一个字符串的字符。最简单的方法就是使用*for...in*遍历。当你这样做的时候实际是在*Character*对象处理；我会在后面的*Character*对象中讲解：
```swift
let s = "hello"
for c in s.characters {
  print(c)
}
```

更深一点，你可以把一个字符串分解为*UTF-8*或者*UTF-16*字节，使用*utf8*或者*utf16*属性：
```swift
let s = "\u{BF}Qui\u{E9}?"
for i in s.utf8 {
  print(i)
}
for i in s.utf16 {
  print(i)
}
```

*string*还有一个*unicodeScalars*属性代表*UTF-32*。为了组合一个*unicodeScalar*，从*number*初始化一个*unicodeScalar*然后拼接到后面。为了说明，下面有一个函数会将两个字符转换为emoji：
```swift
func flag(country: String) -> String {
  let base: UInt32 = 127397
  var s = ""
  for v in country.uincodeScalars {
    s.append(UnicodeScalar(base + v.value)
  }
  return s
}
let s = flag("DE")
```
非常奇怪的是，没有更多的*string*标准方法。但是，比如，你是否需要将一个字符串转化为大写，或者从一个*string*中检索是否包含另一个字符串？大多数现在编程语言会有一个简洁、快捷的方法；但是*swift*没有。原因是这些特性可以由*foundation*框架提供，你在实际编程中会引用这个框架。*String*会桥接给*NSString*。这意味着，很大程度上，*foundation*中的*string*的方法可以由*Swift*中的*String*使用。
```swift
let s = "hello"
let s2 = s.capitalizedString //"Hello"
```
*capitalizedString*属性来自于*foundation*框架；它是cocoa提供的，而不是*swift*。这个是*Nsstring*的属性；相似的，下面是获取一个只字符串位置*string*。
```swift
let s = "hello"
let range = s.rangeOfString("llo")
```
我还没有解释*optional*和*range*是什么，但是上面的代码说明：一个*swift*字符串s变成了一个*NSString*,*NSString*的方法*rangeOfString*被调用，返回一个*Foundation*的*NSRange*的结构体，并且*NSRange*转换为*swift*的*range*且被包装为*optional*。

但是，经常会出现你不需要这样的转换。对于多数原因，你希望仍然使用*foundation*，并且接受*NSrange*。为了完成这样的需求，你需要把你的string转换为*NSString*,使用as
```swift
let s = "hello"
let range = （s as NSString）.rangeOfString("llo")
```
下面是另一个例子同样包含*NSRange*.试想你希望从*hello*中获取*llo*--获取它的第三、第四、第五个字符。*foundation*的*substringWithRange*需要你提供一个*range*--即是你可以使用*foundation*来直接读取*NSRange*，但是这样做的话，编译器会报错：
```swift
let s = "hello"
let ss = s.substringWithRange(1,3)
```

编译器报错的原因是Swift已经包含了*substringWithRange*方法，这里希望你提供一个*swift*的*range*.我后面会解释怎么做，但是你可以通过下面的步骤简单的告诉*Swift*仍然使用*nsfoundation*
```Swift
let s = "hello"
let ss = (s as NSString).substringWithRange(1,3)
```

> 注意：
> swift和cocoa对待String的元素具有不同的处理方式。Swift的概念包含字符。NSString的概念包含*utf-16*。每个设计有自己的优点。和Swift相比，NSString方法更加快速和高效，但是必须遍历这个字符串来探究字符的组成；Swift方式可以提供给你直接的认为的结果。为了强调这个不同，一个非字面意义的Swift字符串没有*length*属性；它有一个类似*NSString*
的*length*的属性是*count*，这个是*utf16*的属性。
幸运的是，在实际中这种情况很少出现；但是可以出现，下面是一个很好的例子：

```swift
let s = "Ha\u{030A}kon"
print(s.characters.count)
let length = (s as NSString).length
```


### 字符（character）
字符对象代表一个单独的unicode族--你认为是字符串的最基本的元素。一个*String*对象可以分解为一组字符对象，通过*characters*属性。正常的，这是个*String.CharacterView*结构体；但是我成它是字符序列。就像我之前提到的，你可以通过*for...in*来遍历一个字符序列来获取字符串中的每一个字符：
```swift
let s = "hello"
for c in s.characters {
  print(c)
}
```
很少碰到*Character*对象会超出他们所属的字符集。甚至没有办法写一个*Character*字面量。为了定义一个*Character*，可以使用单独的String来初始化：
```swift
let c = Character("h")
```
某些情况下，你可以从一个*character*初始化：
```swift
let s = Character("h")
let s = (String(s)).uppercaseString
```
字符可以比较相等；*小于等于*意味着如你希望的。
一个字符串序列有很多的属性和方法。作为一个集合，它具有*first*和*last*属性；这些都是*optional*,因为一个字符串可能为空：
```swift
let s = "hello"
let c1 = s.character.first
let c2 = s.character.last
```
*indexOf*方法定位一个已知字符在一个字符序列中第一次出现的位置，并返回它的index。同样，这是个*optional*因为这个字符可能缺失：
```swift
let s = "hello"
let firstL = s.character.indexOf("l")
```
*swift*中所有的索引从0开始，所以2代表第三个字符。但是这里的索引值并不是Int，我在后面会解释它的类型和这样做的好处。

凭借一个字符串，一个字符串序列包含*contains*方法并且返回*Bool*,来说明是否包含指定的字符：
```swift
let s = "hello"
let ok = s.character.contains("o")
```
相反的，*contains*可以接受一个函数，这个函数携带一个*Character*并返回一个*Bool*.（*indexOf*也可以这样），这段代码反应了这个字符串是否包含元音：
```swift
let s = "hello"
let ok = s.characters.contains{"aeiou".characters.contains($0)}
```
*filter*方法也接受一个函数作为参数，这个函数可以接受一个*Character*并返回一个*Bool*,可以过滤掉那些返回*false*的值。结果是一个序列字符，但是你可以转换这个字符为字符串。因此，下面是删除所有的辅音：
```swift
let s = "hello"
let s2 = String(s.characters.filter{"aeiou".characters.contains($0)}
```
*dropFirst*和*dropLast*方法返回删除第一个或者最后一个字符的序列字符，
```swift
let s = "hello"
let s2 = String(s.characters.dropFirst())
```
*prefix*和*suffix*方法从头或者尾截断指定长度，然后返回新的字符串：
```swift
let s = "hello"
let s2 = String(s.characters.dropFirst())//
```
*split*将一个字符串分割位数组，是一个带有函数作为参数的。在这个例子中，我从字符串中获取词组，这里*word*定义为字符串而不是空格：
```swift
let s = "hello world"
let arr = s.characters.split{$0 == " "}
```
但是，结果是一个数组，为了获得每个字符串对象，我们需要使用*map*函数并且将结果转化为*string*.我会在第四章来讲解，现在你只要相信我：
```swift
let s = "hello world"
let arr = split(s.characters){$0 == ""}.map{String($0}// ["hello","world"]
```
事实上，一个字符串也可以操作些类似*array*的功能。比如，你可以使用下标获取特定位置的字符。不幸的是，这可能不是一个简单的方法，比如，*hello*的第二个字符是什么？下面的代码没有办法编译：
```swift
let s = "hello"
let c = s[1]//编译错误
```

原因是*indexs*在Stirng中是一个特殊的嵌套类型，*String.Index*。这个类型的对象是很滑稽的。以一个字符串的开始位置或者结束位置，或者从*indexOf*返回值；你可以调用*advanceBy*方法来获取字符：
```swift
let s = "hello"
let ix = s.starIndex
let c = s[ix.advancedBy(1)]//e
```
这个复杂的情形是*swift*不知道这个字符序列在哪里，知道遍历过这个序列；调用*advancedBy*就是告诉*swift*这么做的。

除了*advancedBy*方法，你还可以使用*++*或者*--*来增加索引，并且你可以通过*successor*或者*predecessor*来获取下一个或者上一个索引。下面是一个例子：
```swift
let s = "hello"
let ix = s.startIndex
let c = s[++ix]//e
```
或者
```swift
let s = "hello"
let ix = s.startIndex
let c = s[ix.successor]//e
```
一旦你获得指定index上面的字符，你可以使用这个来修改字符串。比如，*insertContentsOf(at:)*可以万一个队列中插入字符--不是字符串中
```swift
var s = "hello"
let ix = s.characters.startIndex.advancedBy(1)
s.insertContentsOf("ey".characters,at:ix)//"hey, hello"
```
相似的，*removeAtIndex*可以删除一个单独的字符。
操作多个字符的时候需要使用*range*，这个是下一节的知识。
注意一个字符序列可以转化位一个包含字符对象的数组--比如，数组Array("hello".characters)这个有时候是值得做的，因为数组的索引是int，因此很方便进行处理。一旦你操作一个字符的数组，你可以将它直接转换为字符串。我会在下下节中给出例子。


### Range
*range*对象代表了一组点。有两个操作符组成range的遍历；你需要提供一个开始开始值和一个结束值，中间是range操作符：

*...*
闭合区间操作符。表达式*a...b*表示从a到b的任何值，包括b

*..<*
半闭合区间。表达式*a..<b*表示不包含b.

在遍历操作符旁边有空格也是合理的。

>警告
>是没有反向的区间：区间的开始值不能比结束值大。

*range*的范围值通常是*number*--大多数情况下：
```swift
let r = 1...3
```
如果区间的结尾是一个负数，必须使用括号包住：
```swift
let r = -1000...(-1)
```

*range*最常用的一个地方就是*for...in*循环中：

```swift
for ix in 1...3 {
  print(ix)
}
```
你还可以使用*range*的实例方法*contains*来检测一个值是不是在这个范围内*range*这个用法实际上是一个interval:
```swift
let ix = //
if (1...3).contains(ix)
```
为了测试目的，*range*的末尾区间也可以是浮点型的：
```swift
let d = //
if (0.1...0.9).contains(0.5)
```
*range*另外一个常见的用途是获取指定位数的字符。比如，这里有一个从字符串中获取第一、二、三位的方法。正如上节我们介绍的，我可以将字符串的字符转换到一个数组中；然后可以使用*range*来获取每个字节，然后转换为String：
```swift
let s = "hello"
let arr = Array(s.characters)
let result = arr[1...3]
let s2 = String(result)//ell
```

相同的，你可以使用*range*来直接的定位一个字符串，但是结果是一个*String.Index*的range，这个我之前说过，比较不容易获取。其中一个让Swift转换*NSRange*的方法是，使用cocoa的方法调用Swift的Range：
```swift
let s = "hello"
let r = s.rangeOfString("llo")
```
你也可以自己生成区间值--比如，使用*String*的*startIndex*的*advancedBy*。一旦你具有一个合适的range类型，你就可以通过下标提取一个字符：

```swift
let s = "hello"
let ix1 = s.startIndex.advancedBy(1)
let ix2 = s.startIndex.advancedBy(2)
let s2 = s[ix1...ix2]//"ell"
```
一个简单的方法是使用字符的*indices*属性，这个可以返回一个半开区间的Range，这个range是从字符的*startIndex*到*endIndex*；你之后可以修该这个range并使用：
```swift
let s = "hello"
var r = s.characters.indices
r.startIndex++
r.endIndex--
let s2 = s[r]//"ell"
```
*replaceRange*方法拼接range，因此可以修改字符串：
```swift
var s = "hello"
let ix = s.startIndex
let r = ix.advancedBy(1)...ix.advancedBy(3)
s.replaceRange(r,with:"ipp")//s is now is "hippo"
```
相似的，可以使用*removeRange*方法删除一串字符：
```swift
var s = "hello"
let ix = s.startIndex
let r = ix.advancedBy(1)...ix.advancedBy(3)
s.removeRange(r)//s is now is "hi"
```
Swift的range和cocoa中的*NSRange*在构成结构是有很大差距的。*Swift*的Range由两个节点定义，Cocoa的*NSRange*是有一个开始点和长度构成，但是你可以将Swift中末尾点是Int的转换为NSRange，并且你也可以使用*toRange*方法将*NSRange*转化为*Swift*的range。
有时候，Swift可以更灵活。比如，当你使用*"hello".rangeOfString("ell")*,Swift可以桥接Range和NSRange，正确的考虑Swift和cocoa不同，还有NSRange的Value是Int，而Range的区间值是String.Index。


### Tuple元组

元组是一个自定义的多个值的集合。作为一个类型，它由包含在括号里的元素组成，这些元素有逗号分割。比如，下面的声明是一个元组，它的元素是*Int*和*String*：
```swift
var pair : (Int,String)
```

元组字面量的表达也是同样的方式--在括号里面包含值，以逗号分开：
```swift
var pair : (Int,String) = (1,"one")
```
元组可以类型推导，因此在声明的时候没有必要显示的声明类型：
```swift
var pair = (1,"one")
```

元组是Swift具有的特征，在cocoa和Oc中并不适用，所以你可以表达cocoa无法表达的数据。在Swift中，元组有很多的使用。比如，元组是一个函数只能返回一个值的很好的解决办法；一个元组是一个值，但是这个值包含其他多个值，所以将元组作为函数的返回值可以使得函数返回多个值。

元组有很多语言上的便利。你可以给一个元组中的元素赋值，就像给多个变量赋值那样：
```Swift
var ix:Int
var s :String
(ix,s) = (1,"one")
```

这个便利的特性可以使你在一行中声明和初始化多个变量：
```swift
var (ix,s) = (1,"one")
```
给一个元组赋值另一个可以安全的交换他们的值：
```swift
var s1 = "hello"
var s2 = "world"
(s1,s2) = (s2,s1)
```

>小结
>当然也有一个全局的函数*swap*来实现交换

为了忽略赋值元组中的某个元素，可以在被赋值的元组中使用下划线代替：
```swift
let pair = (1,"one")
let (_,s) = pair
```
*enumerate*方法可以使你使用*for...in*来遍历。每个遍历的元素的索引和这个元素本身；这两个值组成了元组：
```swift
let s = "hello"
for (ix,c) in s.characters.enumerate() {
  pint("character\(ix) is \(c)"
}
```
在前面我同样指出*numberic*有个实例方法比如*addWithOverflow*返回一个元组。

你可以直接获取元组中每个元素，以两种方式。第一个方法是通过索引，使用数字在元组后面：
```swift
let pair = (1,"one")
let ix = pair.0//now is 1
```
如果你引用的元组不是一个常量，你可以通过同样的方式进行赋值：
```swift
var pair = (1,"One")
pair.0 = 2
```
第二个获取元组元素的方法是给他们定义名字。就像函数的参数，而且必须作为外部声明的一部分。因此，下面是一种声明元组元素名字的方法：
```swift
let pair: (first:Int,second:"one") = (1,"one")
```
或者是：
```swift
let pair = (first:1,second:"one")
```

现在名字作为这个值的一部分，并且可以通过下标来获取。接下来你可以像使用数字那样使用他们：
```swift
let pair = (1,"one")
let pairWithNames: (first:Int,second:String) = pair
let ix = pairWithNames.first//1
```
你也可以将一个元组作为一个函数的参数或者返回值：
```swift
func tupleMaker（） -> (first:Int, second:String) {
  return(1,"one")
}

let ix = tupleMaker().first//1
```
如果你打算在整个程序中使用一个确定类型的元组。给这个元组顶一个名字是一个很好的。使用*typealias*关键字给一个元组定义个类型。比如，在*LinkSame*app中我定义了个*board*类用来操作游戏的布局。*board*是一些对象的，我需要一个方法来描述这些grid的位置，它是一个整型元组，因此我可以这样定义我的元组：
```swift
class Board {
  typealias Point = (Int,Int)
}
```

上面做法的优势是在我的代码中使用*Point*更加的方便。比如，定义一个Point，我可以获取：
```swift
func pieceAt(p:Point) -> Piece? {
  let (i,j) = p
  //
  return self.grid[i][j]
}
```
元组的元素名字和函数参数列表很相似，这不是一个巧合。因为参数列表是一个元组！事实上，每个函数都带有一个元组参数和返回一个元组。因此，你可以传递一个带有多个参数的元组，比如，你有一个这样的函数：
```swift
func f(i1:Int,_ i2:Int) -> () {

}
```
参数列表是一个元组，因此，我可以在调用这个函数的时候传入一个元组：
```swift
let tuple = (1,2)
f(tuple)
```

在上面的例子汇总，f没有外部参数名称。如果一个函数没有外部参数名称，你可以给这个函数传递一个带有元素名字的元组。下面是
```swift
func f2 (i1: i1:Int ,i2:Int) -> () {
}
```
下面是调用：
```swift
let tuple = (i1:1,i2:2)
f2(tuple)
```
但是，这种传递只能是常量，下面的会报错
```swift
var tuple = (i1:1,i2:2)
f2(tuple)//编译错误
```

相似的，*void*函数返回的一个没有值的类型，实际上也是一个元组，只是这个元组没有值，这也是为什么void是（）。

### 可选类型Optional

*Optional*类型是一个enum包装其他任何类型。一个*Optional*只能够包装一个对象。相反的，一个*Optional*可能没有包装任何对象。这使得*Optional*可选：它可能包含另一个对象，也可能不包含。把*Optional*看做一个盒子--这个盒子可能是空的。

下面来创建一个没有包装任何对象的*Optional*。试想下我们想要一个*Optional*包装String类型的*howdy*。其中一个方法是使用*Optional*来初始化建立：
```swift
var stringMaybe = Optional("howdy")
```
如果我们使用print来打印出*stringMaybe*，我们会看到一个表达式和初始化的一致：Optional（"howdy"）

在声明和初始化之后，*stringMaybe*就是一个类型而不是*String*,也不是空白的可选，而是一个包装了*string*的*optional*。这就意味着任何其他包装了*string*的*Optional*都可以赋值给它--但是包装了其他类型的就不可以。下面的代码是合理的：
```swift
var stringMaybe = Optional("howdy")
stringMaybe = Optional("farewell")
```
下面的代码是不合理的：
```swift
var stringMaybe = Optional("howdy")
stringMaybe = Optional("123")
```
*Optional（123）*是包含*Int*的一个optional，你不能给一个包装*String*的*Optional*赋值一个包装*int*的*Optional*。

*Optional*对于Swift非常的重要，使用他们的语法已经内置到语言中。声明一个*optional*的常用方法并不是使用*Optional*的初始化方法获得（当然你可以这么做），而是赋值或者传递某个类型值给一个已经标记位*Optional*。比如，一旦*stringMaybe*被标记为包装String的*Optional*类型，它可以直接的给这个变量赋值字符串。这似乎看起来是不合法的--但是确实是正确的。输出的结果的是赋值的字符串被包装后给我们：
```swift
var stringMaybe = Optional("howdy")
stringMaybe = "farewell"// now StringMaybe 是 Optional("farewell")
```
我们同样需要一个方法来显示的声明一个可以包装String的*Optional*.否则，我们就不能定义一个可选类型的变量，并且不能定义一个具有可选类型参数。通常情况下，一个*Optional*是一个泛型，因此一个包装了String的*Optional*是Optional<String>.我会在在第四章中解释。但是你需要这么写。Swift语法糖支持表达*Optional*类型：在需要进行包装的类型后面添加？。比如：
```swift
var stringMaybe: String?
```
因此我根本不需要*Optional*初始化器。我可以声明一个包装string的可选字符串然后给这个字符串赋值：
```swift
var stringMaybe: String? = "howdy"
```
这实际上是一个常见的创建*Optional*的途径。

一旦你获得包装实际类型的*Optional*，你可以在任何符合这个值类型的地方使用--就像其他类型的使用一样。如果一个函数希望它的参数是一个*Optional*，你可以传递一个可选的作为参数：
```swift
func optionalExpecter(s: String?) {}
let stringMaybe : String? = "howdy"

optionalExpecter(stringMaybe)
```
更重要的是，在包装一个确定类型值的地方，你可以直接传递值。这是因为参数传递就像赋值一样：一个没有包装的值会显示的包装下给你。比如，如果一个函数希望有一个包装了string的参数，你可以传递一个String参数，在收到这个参数的时候会被包装位*Optional*。
```swift
func optionalExpecter(s: String?) {
  //s是一个*Optional*
  print（s）
}
optionalExpecter("howdy")
```
但是你不可以反过来这么做，你不可以给一个不需要*Op*函数传递一个*Op*。
```swift
func optionalExpecter(s: String) {
  //s是一个*Optional*
  print（s）
}
let stringMaybe :String? = "howdy"
optionalExpecter(stringMaybe)
```
错误信息是：可选类型*Optional<String>*的值没有去包装；你是否需要！或者？。你会在Swift中看到很多这样的信息。所以适应他们！正如这些信息建议的，如果你希望在一个不使用*Optional*的地方，你必须去包装--这就是你必须进到包装里面获得这个值，下面我讲解如何做。


#### 给一个可选类型去包装

我们看到不止一种方法将一个对象包装为*Optional*，但是相反过程呢？其中一个方法是使用去包装操作符，这是一个操作符：
```swift
func realStringExpecter(s:String) {}
let stringMaybe : String? = "howdy"
realStringExpecter(stringMaybe!)
```
在上面的代码里面，*stringMaybe!*语法表示获取*Optional*里面内容的操作，获取没有包装的值，由于*stringMaybe*是一个包装了String的可选类型，在可选类型内部是一个字符串。这个里面的内容是函数*realStringExpecter*希望的参数。因此我们可以给这个函数传递一个去包装的值作为这个函数的参数，但是这个参数必须是String。

如果一个可选类型包装了一个确定类型的值，你不可以给包装了这个类型的可选类型发消息。你必须首先去包装，比如，我们获取*stringMaybe*的大写版本：
```swift
let stringMaybe : String? = "howdy"
let upper = stringMaybe.uppercaseString//编译错误
```
解决的办法是给*stringMaybe*去包装获取里面的值。你可以这直接的使用去包装的来获取值：

```swift
let stringMaybe: String? = "howdy"
let upper = stringMaybe!.uppercaseString
```
如果一个可选类型被用在多个需要去包装的地方，并且你每次需要使用去操作符去包装，你的代码可以，比如，在iOS程序中，一个app的window是一个可选：
```swift
//self.window 是包装了UIWindow的一个可选值
self.window = UIWindow()
self.window!.rootViewController = RootViewController()
self.window!.backgroundColor = UIColor.whiteColor()
self.window!.makeKeyAndVisible()
```

上面的操作是愚蠢的，一个很明显的方法是把可选类型的值去包装后直接赋值给一个去包装的变量然后直接使用这个变量：
```swift
//self.window 是包装了UIWindow的一个可选值
self.window = UIWindow()
let window = self.window!
self.window!.rootViewController = RootViewController()
self.window!.backgroundColor = UIColor.whiteColor()
self.window!.makeKeyAndVisible()
```
当然还有另外一种方法，我会在后面解释。

#### 隐式的去包装

Swift提供了另外一个使用*Optional*的方法，你可以声明隐式的声明一个可选类型。这实际上是一种类型--ImplicitlyUnwrappedOptional.一个可选类型，但是编译器限制给它赋值一些指定逻辑；在类型符合的地方可以直接使用可选里面的值，你也可以为这个可选去包装，但是你没有必要，因为它已经隐式的去包装了：比如：
```swift
func realStringExpecter(s:String) {}

var stringMaybe : ImplicitlyUnwrappedOptional<String> = "howdy"
realStringExpecter(stringMaybe)
```
对于可选类型，Swift提供了语法糖来表示隐式的去包装类型。就像一个包装了String类型的可选类型可以写为：String？,一个包装String类型的可选类型去包装可以表示为：String！。因此我们可以重写前面的代码：
```swift
func realStringExpecter(s:String){}

var stringMaybe : String! = "howdy"
realStringExpecter(stringMaybe)//
```
但是你要注意隐式去包装仍然是一个可选类型，它仅仅是便捷了。通过声明一个类型为隐式去包装类型，你可以告诉编译器，如果希望在这个类型符合的地方使用这个值。

只要他们的类型可以强制转换，一个正常的包装类型和隐式去包装类型都可以认为内部转换的：你可以传递他们一个期望的类型。


#### 神奇的nil
到目前我们讨论了包装类型的可选类型。但是如果一个可选类型不包含值呢？这样的一个类型就是之前我们提到的合法的对象，事实上这才构成了整个可选。

你需要一个方法来确认一个可选类型是不是包含值，和一个确定可选没有包装值。Swift通过一个关键字nil使得这两个事情非常的简单：

为了确定一个可选是不是包装值：
  测试可选类型是不是等于nil。如果测试成功，可选类型就是空的。一个空的可选类型打印的结果是nil
确定一个可选没有包装值
  给一个可选类型赋值nil。这个可选类型就没有值了。
  
比如：
```swift
var stringMaybe :String? = "howdy"
print(stringMaybe)//Optional

if stringMaybe == nil {
  print("it is empty")//不会打印
}
stringMaybe = nil
print(stringMaybe)
if stringMaybe == nil {
  print("it is empty")//print
}
```
神奇的关键字*nil*可以使你表达一个概念『包含合适类型的一个可选类型，可以不包含任何的值』。很明显，这是一个很好的；你需要利用这个特点。但是理解nil的神奇之处是非常重要的：在swift中nil不是对象，也不是一个值。他只是一个简语。在实际中思考和使用这个词很正常。比如，当我说一个事物为空的时候，在现实生活中，nil就是什么都没有，nil不代表任何事物。我真正的意思是表达的这个事物和nil相等。

>注意：
>没有包含任何对象的可选类型真正的值是Optional.None。那么一个String类型的可选对象不包含值的真实值就是Optional<string>.None。但是在你实际编写代码的过程中你不需要这么说，因为使用nil更可以清楚的表达你的意思。

因为声明位可选类型的变量可以位nil，因此Swift可以有一个特殊的初始化方法；一个变量声明为Optional默认是nil。下面是合法的：
```swift
func optionalExpecter(s:String?) {}
var stringMaybe : String?
optionalExpecter(stringMaybe)
```

上面的代码很有意思因为上面的代码似乎是不合法的。我么声明了一个变量stringMaybe，但是我们没有给这个变量赋值。至少我们传递它的时候貌似它是具有实际值的。这是因为它确实有值，这个变量已经被赋值了nil.被声明为Optional的变量是唯一一个在Swift被隐式的初始化的。

我们现在意识到Swift中一个中的原则：你不可以给一个包装nil的Optional去包装。这样的可选没有包装任何对象；因此没有可以去包装的值。事实上，在运行的时候显式的给一个没有包装对象的可选去包装会引起闪退：
```swift
var stringMaybe : String?
let s = stringMaybe!//闪退
```
闪退信息：在去包装的时候发现可选值为空。以后我们会经常看到这样的提示。这是很容易出现的错误。事实上，给一个没有值的可选值去包装是Swift中出现闪退的主要原因。你应该习惯这样的错误。事实上，如果你的可选类型没有值存在你经常希望报错，因为它没有值，事实上并不是说你翻来错误。

为了防止这种闪退，你需要保证你的可选类型包含值，如果没有值就不去去包装。一种方法就是检查这个值是不是空的：
```swift
var stringMaybe: String?
if stringMaybe != nil {
  let s = stringMaybe!
}
```
#### 可选链
一个很常见的情况是你希望给一个包装了对象的可选类型发送消息。为了这么做，你需要在某个地方去包装。我在前面给了个例子：
```swift
let stringMaybe : String? = "howdy"
let upper = stringMaybe!.uppercaseString
```

上面的代码格式成为可选链。在点语法之前你已经去包装了。

你不可以给一没有去包装的可选类型发送消息。*Optional*不可以接受任何的消息。
（尽管，他们可以相应一些消息，但是很少见，）如果你给包装在Optional内部的对象发送消息，编译器会报错：
```swift
let stringMaybe : String? = "howdy"
let upper = stringMaybe.uppercaseString//编译错误
```
但是，我们已经看到，如果你给一个不包装任何对象的Optional去包装就会闪退。所以你如何确定一个Optional是不是包含值呢？在这种情况下又如何发送消息：Swift提供了一种特殊的语法来满足这样的需求。为了给一个可能为空的Optional发送消息，你可以选择性的给这个可选去包装。在可选变量后面使用？：
```swift
var stringMaybe : String?
let upper = stringMaybe?.uppercaseString
```
这个可选链中你使用了？来选择性的去包装。通过这个语法，你可以选择性的去包装--也可以说有条件的。条件就是其中之一是安全的；对nil的检测结果告诉了我们。我们的代码的意思：
如果stringMaybe包含值，给它去包装然后给它发送消息，否则不要去包装也不要发送消息。

这样的代码是一把双刃剑，一方面，如果stringMaybe是nil，你不会闪退。另一方面，如果StringMaybe是个nil，这行的代码不会做任何事情--你不会得到一个大写的字符串。

但是现在有一个新的问题。在上面的代码中，我们定义了一个upper变量，这个变量会接受一个uppercaseString的消息。现在证明，这个消息可能不会传递。那么实际upper是如何初始化呢？

为了满足这种情况，Swift有一个特殊的原则。如果一个可选链包含一个未去包装的可选对象，并且如果这个可选链产生一个值，那么这个值也是一个可选。因此，upper是一个包装String的可选对象。这个机制非常聪明，因为它包含了两种情况。让我们来讨论下：一种，stringMaybe包含值：
```swift
var stringMaybe :String?
stringMaybe = "howdy"
let upper = stringMaybe?.uppercaseString//upper 的类型是string？
```

上面的代码得到的upper不是一个String；它不是『howdy』，它是一个包装了『HOWDY』的一个可选；另一方面，如果试着去包装失败，可选链就会返回nil：
```Swift
var stringMaybe:String?
let upper = stringMaybe?.uppercaseString// upper是一个nil的可选
```
给一个可选类型以这样的方式去包装是优雅安全的；但是考虑到便捷性、从另一个方面，及时stringmaybe是nil，我们在运行时也不会崩溃。另一方面，我们比之前更好 了，我们最后会得到另一个可选类型。不管stringMaybe是不是为空，upper都是一个可选，因此要使用这个值，我们必须去包装。我们也不知道upper是不是空，所以我们又碰到了之前的问题。我们许熬确定这个对象是安全的，然后才能去包装。

多个可选链是合理的。他们如你想象的那样；在可选链中不管有多少个可选，如果他们任何一个如果没有成功去包装，那么最后得到的结果就是一个包装了这个类型的可选，比如
```swift
let f = self.window?.rootviewController?.view.frame
```
一个View的frame属性是一个CGrect。但是在这个代码之后，f不是一个CGRect，它是一个包装了Cgrect类型的可选。如果这个可选链中任何一个去包装的地方失败了，那么最后的结果就是返回nil的包装类型。

>tip:
>观察上面的代码最后并没有嵌套；因此不会产生一个包装了CGREct的可选又被包装的情况，因为这两个可选在可选链中都被去包装过。但是，有其他的情况，会出现这个情况，我会在第四章讲解。

如果一个包含未去包装的可选产生了一个结果，你可以通过检查最后的结果检测可选链中的可选是不是安全的去包装；如果值不为nil，这说明都去包装成功。但是如果有一个去包装失败就不会产生结果？比如：
```swift
self.window?.rootviewController = UIViewController()
```
现在我们困惑了事实上，肯定不会闪退；如果self.window是nil,它是不会被去包装的，所以我们是安全的。但是如果self.window是nil的，我们也不可以正确的给rootviewcontroller赋值！因此在这个可选链中知道是不是去包装成功是非常完美的。幸运的是，这里有个途径可以找到。在Swift中每个语句要么返回一个值，要么返回Void。因此，给一个可选链赋值一个未去包装的可选类型最后返回一个包装Void的可选--并且你可以捕获这个可选。这就意味着你可以检测这个可选类型是不是nil；如果不是nil，那么赋值就成功。比如：
```swift
let ok: Void? = self.window?.rootViewController = UIViewController()
if ok != nil {
  //it work
}
```
自然的，你不需要显示的去包装获取一个变量的值；你可以在一行中获取检测这个值：
```swift
if (self.window?.rootViewController = UIViewController()) != nil {
  //it work
}
```

如果调用一个函数返回一个可选，你可以给这个值去包装然后使用。你不要为了这个做给一个值去包装；你可以在某个地方给这个值去包装，然后在这个函数调用后面放一个！或者？。这样和我们之前做的没有什么不同，除了一个可选属性或者变量，这个是一个函数返回一个可选。比如：
```swift
class Dog {
  var noise:String?
  func speak()->String? {
    return self.noise
  }
}
let d = Dog()
let bigname = d.speak()?.uppercaseString
```

这段代码之后，不要忘记，bigname是一个包装了String的可选类型。
>注意
>在第五章我会讨论一些Swift语法来判断一个Optional是不是nil


####可选类型的比较
在和非Optional对象进行比较的时候，Optional有个特殊的对象：比较的是去包装后的对象，而不是可选类型本身，所以比如：
```swift
let s :String? = "Howdy"
if s == "Howdy"
{
  //结果是相等的
}
```
这按道理应该不等的，但是结果确实相等--由于必须给一个可选去包装后再和比较对象进行比较（特别是必须要检测一个可选是不是nil）。上面的例子中不是比较包装了"Howdy"的可选类型，Swift自动的比较里面的包装对象。如果Optional里面包装的对象不是"Howdy"，比较结果就会失败。如果里面没有包装对象，比较也会失败。因此，你可以让s和nil或者一个字符串进行比较。
同样的，如果一个可选包装的类型可以使用<等符号进行比较，这些比较可以直接使用在可选上：
```swift
let i : Int? = 2
if i<3 {
  //ok
}
```


####为什么是可选？
既然你知道了如何使用可选类型，你应该好奇为什么使用可选。为什么Swift具有可选？这样有什么好处？

可选一个重要的作用就是提供了一个和OC对象值的一个交换。在OC中，任何对象可以是nil。因此，在OC中，你需要一种方式给OC发送nil或者从oc接受nil消息。Swift给你提供了唯一的方式。

Swift通过Cocoaapi自动的使用合适的类型来帮助你。比如，考虑UIView的backgroundColor属性，这是一个UIColor，但是也是可能位nil，并且也运行你给这个值设置nil。因此，它的类型是UIColor?.为了设置这样的值你不需要直接的使用可选类型！记住，给一个可选类型赋值包装类型是合法的，因为被赋值的值会给你包装好，因此，你可以直接给你的设置myView.backgroundColor = UIColor或者nil。但是如果获取一个UIView的背景颜色，你现在就获得一个包装了UIColor的可选类型，你必须要清楚这个事实，所有的原因我已经讨论过：如果你不这样做，注意这个：
```swift
let v = UIView()
let c = v.backgroundColor
let c2 = c.colorWithAlphaComponent(0.5)//编译错误
```
但是，大多数情况下，Cocoa的类型对象不会被标记为Optional。这是因为，尽管理论上是可以为空的（因为Oc可以为nil）。在实际中不会这么做。因此Swift给你留了一步来把值作为它本身来看待。这个魔法通过Cocoa API来完成。在Swift的第一个公开版本中，所有从Cocoa来的对象都是被标记为Optional（通常是隐式的被包装为）。但是后来apple使用了hand-tweaking来表示那些不需要标记位可选的。

在极少数情况下，你还会在Cocoa API中遇到隐式的去包装的情况。比如，API NSBundle的一个方法loadNibNamed:owner:options:
```swift
func loadNibName(name:String!,owner:AnyObject!,options:[NSObject:AnyObject]!)-> [AnyObject]!
```
这些隐式去包装的类型说明这些方法还没有被hand-tweaked.他们不能准确的代表这种情况--你永远不会给第一个参数传递nil，比如--。

另外一个可选的重要用处就是初始化一个对象属性。如果一个变量被声明为Optional，它本身就有一个值即使你不给它赋值--默认是nil。这会用在有些对象必须具有初始值，但又不是立刻。一个典型的例子是，outlet,它代表了引用IB上的一个对象：
```swift
class ViewController: UIViewController {
  @IBOutlet var myButton: UIButton!
}
```
从现在开始，@IBOutlet在内部连接Xcode，重要的是，































