# 引言:函数式编程基础

理解编程行为主要有两种方法。第一种，也是历史上较常见的一种观点,认为程序员向计算机提供一系列指令，以使其按某种方式运行。这种编程模型将程序员与一种特定的编程工具——即计算机——的设计联系在一起。在这种风格的编程中，计算机是一种设备，它接收输入，访问内存，将指令发送到处理单元，最后将输出传递给用户。这个计算机模型被称为冯·诺依曼架构，以著名数学家和物理学家约翰·冯·诺依曼的名字命名。

最能体现这种程序思维方式的编程语言是C语言。C语言从操作系统控制的标准输入接收数据，在物理内存中存储和检索必要的值，最后通过操作系统控制的标准输出返回所有输出。程序的内存管理精妙而繁琐,还需要处理指向特定内存块的指针，程序员往往需要高强度注意,并手动编写管理代码.在编写C程序时，程序员对手头问题的理解必须落实到具体的执行流程,以符合面前计算机的物理体系结构.

但是冯·诺依曼架构的计算机并不是执行计算的唯一方法。人类执行着各种各样的计算，这些计算与内存分配和指令集都没有任何关系:对书架上的书进行排序，求解微积分函数的导数，给朋友指路，等等。当我们编写C代码时，我们写的只是 _计算的一种特定的实现_ 。创建Fortran的团队的领导者约翰·巴克斯(John Backus)在他的图灵奖演讲中问道:“编程能不能从冯·诺依曼风格中解放出来?”

这个问题引出了第二种理解编程的方式，这也是本书第一单元的主题。**函数式编程** 试图将编程从冯·诺依曼风格中解放出来。函数式编程立足于抽象的数学计算概念，而数学概念是超越特定实现的。由此有一种编程方法，它解决问题的方式就是直接把问题描述出来。通过专注于 _计算_ ，而不是 _计算机_ ，函数式编程允许程序员使用强大的抽象，使许多具有挑战性的问题更容易解决。 

这样做的代价是，起步阶段会更加困难。函数式编程的思想通常是抽象的，我们必须始于从这些抽象思想构建程序。在构建有用的程序之前，我们需要学习许多概念。在学习第一单元时，请记住，你正在于 _超越计算机_ 的维度进行编程.

正如C几乎完美地体现了冯·诺依曼风格的编程，Haskell是你能学到的最纯粹的函数式编程语言。作为一门语言，Haskell完全致力于实现巴克斯的梦想，不允许你回到更熟悉的编程风格。这使得学习Haskell比许多其他语言更难，但是学习Haskell使你不得不深入了解函数式编程。在本单元结束时，你将打好函数式编程基础，以贯通浏览所有其他函数式编程语言，并为学习Haskell做好准备。

# 函数与函数式编程

学习了第2课,你将能够:

- 了解函数式编程的总体思路
- 在Haskell中定义简单的函数
- 在Haskell中定义变量
- 理解函数式编程的益处
 
学习Haskell时，你需要了解的第一个主题是，什么是函数式编程?众所周知，函数式编程是一项极具挑战性的课题。虽然确实如此，但函数式编程的基础出乎意料地简单。你首先要了解的就是:函数式编程语言中的函数意味着什么。之前你可能已经大概理解了使用函数的含义。在这一课中，你将看到Haskell中函数必须遵守的简单规则，这些规则不仅使你的代码更容易理解，还会带来全新的编程思维方式。

> **考虑这个** 你和你的朋友去买披萨,菜单上不同大小的披萨价格不同:
> 1. 18 英寸, $20
> 2. 16 英寸, $15
> 3. 12 英寸, $10
> 你想要知道哪个选项能按没块钱买到最多的披萨,你可能要定义一个函数,算出每平方英寸披萨的价格.

## 函数

函数到底是什么?这是理解函数式编程的重要问题。Haskell中函数的行为直接来源于数学。在数学中，我们经常说\\(f(x) = y\\)，这意味着有一个函数\\(f\\)，它接受一个参数\\(x\\)，并映射到值\\(y\\)。在数学中，每个\\(x\\)都可以映射到一个且只能映射到一个\\(y\\)。如果给定函数\\(f\\), \\(f(2) = 2,000,000\\)，那么\\(f(2) = 2,000,001\\)的情况永远不会发生。

有想法的读者可能会问:“平方根函数呢?4有两个根，2和-2，那么\\(\sqrt{x}\\) 显然指向两个\\(y\\)，它怎么可能是一个真正的函数呢?”关键是要意识到\\(x\\)和\\(y\\)不一定是同一个东西。根号\\(x\\)是正根，所以\\(x\\)和\\(y\\)都是正实数，问题不就解决了。但\\(\sqrt{x}\\)也可以是一个从正实数到实数对的函数。在这种定义，每个\\(x\\)正好映射到一个对。
在Hskell中,函数就像数学中那样工作.下图表示了一个定义为`simple`的函数:

![](pic/2-1-function.svg)


这个`simple`函数接受一个参数`x`，然后原封不动地返回这个参数。注意，不同于许多其他编程语言，在Haskell中不需要指定"返回一个值"。因为Haskell认为函数必须返回一个值，而不必再说明这一点。你可以将你的`simple`函数加载到GHCi中并查看它的行为。要加载一个函数，你所要做的就是把它放在一个文件中，并在GHCi中使用`:load <filename>`:
```
GHCi> simple^2
2
GHCi> simple "dog"
"dog"
```

> 注意:本章中,我们将使用 GHCi——Haskell的读取-求值-输出循环
 (REPL)——来运行命令,求取结果.

为了使函数的行为类似于数学中的函数,Haskell迫使所有函数都遵循三条规则:

- 所有函数必须接受一个参数.
- 所有函数必须返回一个值.
- 不论何时,以同样的参数调用函数,其必须返回同样的结果.

这三条规则是数学上函数的基础定义的一部分.当"同输入同输出"的规则应用于编程语言中的函数时,它往往被叫做 **引用透明性** .

> 译注: 我要强调前两条规则中的 "一个",因为Haskell严格意义上没有输入多参数和返回多值的函数.我们会在其他地方见到这一点.

## 函数式编程

如果函数只是\\(xs\\) (\\(x\\)的复数形式)到\\(ys\\) (\\(y\\)的复数形式)的映射，那么它们与编程有什么关系呢?在20世纪30年代，一位名叫Alonzo Church的数学家试图创建一个只使用函数和变量(\\(xs\\)和\\(ys\\))的逻辑系统。这种逻辑系统被称为**lambda演算**。在lambda演算中，你把所有东西都表示为函数:true和false，甚至所有的整数。

Church最初的目标是解决数学中集合论领域的一些问题。不幸的是，lambda演算并没有解决这些问题，但Church的工作带来了更有趣的东西。事实证明，lambda演算足以成为一种通用的计算模型，等价于图灵机!


> **图灵机是什么?**
>
> 图灵机是由著名的计算机科学家艾伦·图灵开发的计算机的抽象模型。从理论的角度来看，图灵机是有用的，因为你可以用它研究什么可以计算，什么不能计算，不仅仅是在数字计算机上，而是在任何可能的计算机上。这个模型还被计算机科学家用于展示计算系统之间的等价性,只要这些系统各自能够模拟一个图灵机出来。例如，你可以用它来表明，没有什么东西是用Java能计算，而汇编语言不能计算的。

lambda演算和计算之间的关系被称为Church-Turing论题(更多信息，请参见[www.alanturing.net/turing_archive/pages/reference%20articles/the%20Turing-Church%20Thesis.html](https://www.alanturing.net/turing_archive/pages/reference%20articles/the%20Turing-Church%20Thesis.html))。这个发现的奇妙之处在于，你有了一个数学上可靠(sound)的编程模型!

你使用的大多数编程语言都是工程上的杰作，但对于程序的行为却几乎没有保证。有了数学基础，Haskell能够从您编写的代码中剔除一整类疏漏和问题。编程语言的前沿研究正在尝试以各种数学方法证明程序完全符合你的预期。此外，大多数编程语言都不是按数学方式设计的,因此你使用的抽象方式会受制于该语言中的工程取舍。如果你能定义程序(而不是编写程序)，你就能证明关于你代码的东西，并能达到数学所允许的几乎无限的抽象层次。这就是函数式编程的目标:以一种可用的方式将数学的力量带给程序员。

## 函数式编程的实践价值

对编程进行数学建模有很多实际意义。因为上述的三条规则:所有函数都必须接受和返回值，并且对同一个参数必须始终返回相同的值，所以Haskell是 _安全的_ 编程语言。这个安全指的是程序的行为总是完全符合你的期望，并且你可以很容易地推断出它们的行为。一种安全的编程语言能够强制你的程序按照预期的方式运行。

让我们来看看不安全的代码，违反了简单的函数规则。假设你正在阅读一个新的代码库，并遇到类似下面的代码行。
> 函数调用中的隐含状态
```javascript
tick()
if(timeToReset){
  reset()
}
```
这段代码显然不是Haskell，因为`tick`和`reset`都违反了我们建立的规则。这两个函数都不接受任何参数，也不返回任何值。问题是，这些函数在做什么，它和Haskell的函数有什么不同?我们可以很容易地假设tick正在增加计数器的值，并且reset会将计数器恢复到它的初始值。即使我们mz完全猜中，这种推理也给了我们对问题的洞察。如果函数没有接受参数，那就是在环境中访问值;如果没有返回值，那就是在环境中修改值。而在环境中改变一个值，就是改变程序的状态,在代码中产生副作用，进而这些副作用使代码难以推理，因此说这段代码不安全.

很可能`tick`和`reset`都访问了一个 _全局变量_ (在程序中任何地方都能访问到的变量)，这在任何编程语言中都被认为是糟糕的设计。但是副作用使得即使是最简单、编写良好的代码也很难推理。举个例子，我们来看一个值的集合`myList`，并使用内置的功能对它进行反转。下面的代码是有效的Python、Ruby和JavaScript代码。看看你能不能弄清楚它做了什么。

> 标准库函数的模糊行为描述
```python
myList = [1,2,3]
myList.reverse()
newList = myList.reverse()
```

那么，你希望newList的值是什么?因为这在Ruby、Python和JavaScript中都是有效的程序，所以假设newList的值相同也可以吧?以下是这三种语言的答案:
```
Ruby -> [3,2,1]
Python -> None
JavaScript -> [1,2,3]
```

对于三种语言中的完全相同的代码，有三个完全不同的答案!在调用`reverse`时，Python和JavaScript都有副作用。因为调用`reverse`的副作用在每种语言中都是不同的，而且对程序员来说是不可见的，所以两种语言给出的答案也不同。这里Ruby代码的行为类似于Haskell，而且没有副作用。这里可以看到引用透明的值。使用Haskell，您总是可以看到每个函数具有哪些效果。当你调用`reset`和`tick`函数时，它们所做的更改对你来说是不可见的。不查看源代码，您无法确切知道他们正在使用和更改哪些甚至多少个其他值。Haskell不允许函数有副作用，因此所有的Haskell函数都必须接受一个参数并返回一个值。如果Haskell函数不总是返回值，它们就必须改变程序中的隐藏状态;否则，它们就没用了。如果它们不接受参数，就必须访问一个隐藏的参数，这意味着它们不再是透明的。

Haskell函数的这个特性使代码更容易预测。即使在Ruby中，程序员也可以使用副作用。当使用其他程序员的代码时，你仍然不能假设调用函数或方法时发生了什么。因为Haskell不允许这样做，所以您可以查看任何程序员编写的任何代码，并推断其行为。

> 小测: 许多语言都使用 `++`运算符来增加一个值.例如 `x++`就增加了 `x`. 你觉得 Haskell 有没有表示这种行为的运算符或函数?
> <details><summary>答案</summary> C++之类语言中使用的<code>++</code>运算符不可能存在于Haskell中，因为它违反了函数的数学规则。最明显的规则是，每次对变量<code>a</code>调用<code>++</code>时，结果是不同的。</details>

### 变量(Variables)

> 译注 称其为"变量"有些奇怪,因为Haskell中被叫做"变量"(variable)的东西只能被定义一次,并且一经定义就不再能修改.接下来马上就要讲到了
> 
Haskell中的变量很简单。这样将变量x赋值为2:。

> 定义你的第一个值
```haskell
x = 2
```

Haskell中变量的唯一问题是它们根本不是变量!如果尝试编译下面这段Haskell代码，就会出现错误。

> Variables aren’t variable!
```haskell
x = 2
x = 3 -- 不可能通过编译,因为 x 被修改了
```

Haskell中更好的理解变量的方式是把它当成 _定义_ 。再一次，你看到数学思维取代了你通常思考代码的方式。问题在于，在大多数编程语言中，变量重赋值对于解决许多问题都至关重要。那么为什么Haskell禁止这么做?为了引用透明性。这确实是一个严格的规则，但也有回报:你始终知道调用一个函数后，世界不曾变过。

> 小测: 即使在没有`++`运算符的语言中，也可能有`+=`运算符，它通常用于增加一个值。例如，`x += 2`使`x`增加2。你可以把`+=`看作一个遵循我们规则的函数:它接受一个值并返回一个值。这是否意味着`+=`可以在Haskell中存在?
> <details><summary>答案</summary> 不可能.对一个变量多调用几次,<code>+=</code>首先它的值被改变了,其次每次调用也会产生不同的结果.</details>

在编程中使用变量的主要好处是使代码更清晰，避免重复。例如，假设你想要一个名为`calcChange`的函数。这个函数有两个参数:欠多少钱和给多少钱。如果给了你足够的钱，你就退还差额。但如果你没有得到足够的钱，你不会想给负的钱;你会返回0。这里有一种写法。

> calcChange v.1
```haskell
calcChange owed given = if given - owed > 0
                        then given - owed
                        else 0
```

这个函数有两个问题:

- 即使是一个很小的函数，它也很难阅读。每次你看到表达式 \\(given - owed\\)，你就必须思考发生了什么。如果表达式比减法更复杂,这就不好看了。

- 这是在重复计算!减法是一种廉价的操作，但如果这是昂贵的操作，则会浪费不必要的资源。

Haskell 用一个特殊的`where`结构来解决问题,这是用`where`结构重塑的函数:

> calcChange v.2
```haskell

calcChange owed given = if change > 0
                        then change
                        else 0
  where change = given – owed -- given - owed 只被计算一次,
                              -- 而后赋值给 change
```
你首先会感到有趣的是，`where`子句颠倒了写变量时的顺序。在大多数编程语言中，变量都是在使用之前声明的。这种约定主要是程序语言允许改变状态而带来的副产品。变量顺序之重要，是因为你总是可以在赋值之后重新赋值。在Haskell中，由于引用透明性，这不是问题。Haskell方法还有一个可读性方面的好处:如果你阅读算法，就很明显能看出意图。

> 小测:填充这个`where` 子句中的内容:
> ```haskell
> doublePlusTwo x = doubleX + 2
>   where doubleX = __________
> ```
> 
>  <details><summary>答案</summary> <code>doubleX = x*2</code></details>

### GHCi 中的可变变量

因为变化是生活中不可避免的一部分，所以有时给变量重新赋值是有意义的。其中一种情况发生在Haskell REPL (GHCi)中。在GHCi中，你可以重新给变量赋值。下面是一个例子:

```
GHCi> x = 7
GHCi> x 
7
GHCi> x  = [1,2,3]
GHCi> x
[1,2,3]
```


在GHC版本8之前，GHCi中的变量需要以`let`关键字开头，以标记它们不同于Haskell中的其他变量。如果你喜欢，你仍然可以在GHCi中使用`let`来定义变量:
```
GHCi> let x = 7
GHCi> x
7
```

注意,这种方法也可以用来定义单行小函数: 
```
GHCi> let f x = x^2
GHCi> f 8
64
```



在Haskell的其他一些特殊环境中，您将看到let以这种方式使用。这可能会令人困惑，但这么做主要是为了如实描述现实世界的任务,以使其好懂.

要认识到，GHCi中能够改变变量的定义是一种特殊情况。尽管Haskell可能很严格，但如果每次你想尝试不同的变量时都必须重启GHCi，那就太反人类了。

> 小测: 以下代码中x的最终结果是什么?
>```
> GHCi> let x = simple simple
> GHCi> let x = 6
> ```
>  <details><summary>答案</summary> 因为你能给变量重新赋值,x最终的值是6.
> </details>


## 总结

在这一课中，你了解了介绍函数式编程,学习了用Haskell编写基本函数。你学到了，函数式编程对函数的行为进行了如下限制:

- 函数必须始终接受一个实参。
- 函数必须始终有返回值。
- 使用相同的参数调用同一个函数，必须始终返回相同的结果。

这三条规则对你使用Haskell编写程序的方式有深远的影响。以这种风格编写代码的主要好处是，你的程序更容易推理，行为也更可预测。

接下来看看这些问题:

**Q2.1** 使用Haskell的`if then else`表达式来编写`calcChange`。在Haskell中，所有的`if`语句都必须包含`else`分支。那为什么不能单独使用if语句呢? 结合函数的三条规则回答一下.

**Q2.2** 编写名为`inc`、`double`和`square`的函数，分别将参数`n`递增、乘以2和平方。

**Q2.3** 写一个函数，接收一个值`n`。如果`n`是偶数，函数返回 `n - 2`，如果数是奇数，函数返回`3 × n + 1`。要检查这个数是否为偶数，可以使用Haskell的`even`函数或`mod` (Haskell的取模函数)。

 
