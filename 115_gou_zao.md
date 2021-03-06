# 1.15 构造
##构思
你现在知道了什么是对象，什么是实例。那么你是程序需要什么类型的对象呢，这些对象又都有哪些属性和方法呢，他们什么时候被初始化，你拥有这些实例之后又能做些什么？非常的不幸，我没办法告诉你这些。这是一门艺术--关于面向对象的艺术。我能告诉你的是你的主要点是实现你的面向对象的程序--这个过程我称为变为一个程序。

面向对象编码必须对对象的本质有一个深刻的理解。你想设计一个对象类型，这个对象封装了一系列的方法和属性。然后当你初始化这些对象类型的时候，你需要确认你的实例具有正确的声明周期，合理的给其他对象提供了接口，合理的和其他对象进行沟通。

**对象类型和APIs**

你的程序应该有很少量的顶层函数和变量（全局函数和变量）。类的方法和属性--特别是实例方法和属性--是多数的行为。类给每个实例特殊的能力。它同样可以帮助你把你的代码组织的更加的合理和可维护。

我们从两个方面总结对象的特征：封装函数和维持状态。

* **封装函数**


> 每个对象都有自己的角色，并且面向现实生活--对于其他对象，对于某些程序猿确实是--都是一个不透明的墙，唯一的入口就是这样方法，这些方法可以承诺相应，操作可以执行，当正确的消息发送过来的时候。后面实现这些操作详细是在内部；没有其他对象需要知道。

* **保持状态**

> 每个单独的实例都是它保存的数据的一个集合。通常情况下，data是私有的，所以它也是被封装的；其他对像不需要知道这个数据是什么或者它在哪个表里保存。唯一从外部可以发掘对象内部的数据的方法是通过公开的方法或者属性获取。

下面的例子，画了一个对象，它的工作是实现一个栈--它有可能是一个**Stack**类的实例（对象）。栈是一个数据结构，这个结构使用**LIFO**来保存数据。它可以相应两个方法：**push**和**pop**。**push**意味着给这个集合中添加数据。**pop**意味着从这个集合中移除最后进入栈中的数据。这就像一个模板的集合：在栈上面放着多个模板，移除的时必须一个一个来，所以最底下的只能到最后一个移除：

![1-3](屏幕快照 2016-05-11 15.17.18.png)
栈的例子说明了函数的封装，因为外面的世界不知道栈里面实际是如何实现的。它可能是一个数组，也可能像一个索引，或者是其他的实现。但是使用这个栈的对象--就是发送push或者pop消息的对象--并不关心它是如何实现的，这对程序猿来说是好事，因为作为一个开发者，可以安全的给他人提供一个接口，而不用担心会破坏内部的结构。从另一个角度来说，栈对象不需要关心是谁发出的push或者*pop*消息。

栈对象也说明了状态的保持，因为它不仅仅是一个栈数据的大门--它也是一个栈数据。其他对象可以或者这个数据，但是只能通过栈对象本身，而且必须获得栈的允许。栈数据有效的存在于栈对象的内部；其他对象获取不到。其他对象可以做到的只能是**push**或者**pop**。如果此时一个对象处于我们栈的顶层，任何对象发送**pop**消息给这个栈对象都会收到返回的对象。如果没有对象给这个栈对象发送**pop**消息，这个对象会一直在栈的顶部等待。

一个对象类型有资格被其他对象使用的的消息总数--APIs--就像一个你要求这个对象类型坐等事情的列表。你的对象类型区分了你的代码；他们的APIs在这些模块之间构成了基本的交流。

在现实生活中，当你编写程序的时候，你是使用的大量的对象类型是苹果而不是你的。Swift本身只有少量的对象类型。比如**String**、**Int**；你还是需要使用*import UIKit*，这里面包含了大量的对象类型，这些大量存在你的代码中。你不需要创建任何这样的类型，只需要学习如何使用他们，你可以学习这些APIs,比如文档。苹果的官方文档为每个对象类型提供了大量的纸张来说明它的属性和方法。比如，想知道NSString类有哪些方法，你可以在文档中搜索NSString。文档里有大量的属性和方法，可以告诉你NSString对象可以做什么。里面详细讲解了所有关于NSString的内容。

苹果在你写代码之前已经为你做了大量的工作，结果就是你只需要使用苹果提供的对象类型。你也可以创建新的对象类型，但是这样做意义不大。

**实例创建，作用域，声明周期**

Swift程序中最重要的实体就是实例。对象类型定义了不同实例的轮廓和实例的行为。但是这些对象的实例是不同事物的状态保持者，这个是程序关心的，还有发送给实例的实例方法和属性。所以程序做任何事情都需要实例。

但是默认情况下，是没有实例的！回看例子1-1，我们定义了一些类型对象，但是并没有他们的实例。如果我们运行程序，我的类型对象会存在，但是仅仅是存在，我们已经创建了一个有潜力的世界--一些对象的类型可能存在。在这个世界，任何事情都可能发生。

实例不会莫名其妙的产生。你必须实例化一个类型来获得这个实例。因此你程序的大部分操作由初始化实例构成的。当然你也希望你能够持有这些实例，所以你会把每一个新实例话的实例赋值给你变量来持有它。实例的生命周期是由引用这个实例的变量决定的。实例对其它实例的可见是由引用它的变量的作用域决定的。

大部分面向对象的艺术被证明无外乎这些，给实例一个生命周期，让他们可以被其他对象访问。你经常会将一个实例放到一个特殊的盒子里--把它赋值给一个声明在一定作用域的特殊的变量--感谢变量的声明周期和作用域，当一个对象还需要的时候这个变量会被长期持有，这样的话其他的代码可以通过引用这个实例和它对话。

规划如何创建一个对象，解决对象的生命周期和让对象之间进行沟通听起来令人胆怯。幸运的是，在现实生活中，当你编写iOS程序的时候，Cocoa的库给你提供了初始的框架。

比如，对于一个iOS应用，你从一开始就知道你需要一个*app delegate*类型和一个控制器类型，事实上，在你创建iOS应用的时候，Xcode已经为你提供了。更重要的是，当你的程序启动，**runtime**会为你初始化这些类型，并且把这些类型方法放到合理的位置。**runtime**会创建一个*App Delegate*并且保存在整个App生命周期内；它会创建一个**window**实例并赋值给**Delegate**的属性；它会创建一个控制器实例并赋值给**window**属性。最后，控制器实例还有**view**属性--会自动的出现在**window**中。

因此，不需要做多少工作，你已经创建了一些对象生命周期是整个app，包括一个组成你的可见窗口。同样重要的是，你很好的创建了全局的引用这些对象。这意味着，不需要写任何代码，你已经获得了一些重要的对象，同时也有了一个初始地方来存放你其他的对象需要的界面。

**总结和结论**

当我们构想一个面向对象程序去解决特定任务的时候，我们脑子里有一个对象的本质。他们是类型和实例。一个类型是一个方法的集合，这些方法描述了所有实例可以做什么（函数封装功能）。相同类型的实例不同的是他们属性的值（状态的维持）.对象是一个组织的工具，是一系列封装了指定任务代码的盒子。对象同样是概念工具，程序猿必须把大的程序分解为小的任务，每个任务赋值给合适的对象。

同时，没有任何一个对象是孤立的，对象可以相互合作，概括的说可以相互交流--就是通过向另一个发送消息。几行的交流可以被安排为多中。对象之间的协作和组织是面向对象的一个重要挑战。在iOS程序中，你通过Cocoa框架提高，它提供了一些初始的对象和基本的构想艺术。

使用面向对象写出一个你想要的干净、可维护的代码本身就是一个艺术；你的能力将随着锻炼提升。甚至你会阅读一些关于如何高效的组织和重构代码的面向对象书籍。


