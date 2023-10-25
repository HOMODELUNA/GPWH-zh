# Haskell 上手
学习了第1课,你将能够:

- 为 Haskell 开发安装好工具
-  使用 GHC 和 GHCi
-  Use tips for writing Haskell programs

## 欢迎来到 Haskell!

在深入学习Haskell之前，您需要熟悉您将在旅程中使用的基本工具。本课将引导您接触Haskell。这一课从下载编写、编译和运行Haskell程序的基础知识开始。然后，请阅读示例代码，并思考如何用Haskell编写代码。千里之行,始于足下.

### Haskell Platform
学习一门新的编程语言最糟糕的部分是第一次搭建开发环境。幸运，甚至有些惊讶的是，这在Haskell中根本不是问题。Haskell社区已经整合了一个单一的、开箱即用的工具包，称为Haskell Platform.

> 译注: 不幸的是,Haskell Platform 自2022年就已经[被弃用了](https://www.haskell.org/platform). 目前(2023年)Haskell社区推荐使用[ghcup](https://www.haskell.org/ghcup)来管理Haskell版本

Haskell Platform 包含如下组件:

- Glasgow Haskell 编译器 (GHC)
- 交互性解释器 (GHCi)
- 包及项目管理器stack
- 一批常用的Haskell包 

Haskell Platform 可以从www.haskell.org/downloads#platform下载。从这里开始，按照所选操作系统上的说明进行安装。本书使用Haskell 8.0.1或更高版本。

### 代码编辑器

安装好Haskell 之后，你可能想知道应该使用哪种编辑器。Haskell是强烈鼓励你\textks{在动手之前先思考}。因此，Haskell程序往往极其简洁。除了管理缩进和提供语法高亮之外,编辑器帮不到你多少。许多Haskell开发人员将Emacs与Haskell模式一起使用。但是，如果您还不熟悉Emacs(或不喜欢使用它)，那么你不必特意学习Emacs。我的建议是，无论你最常用的编辑器是什么，你都应该配备Haskell插件。本书中简单的文本编辑器就够用了，如Pico或notepad++，而且大多数成熟的ide都有现成的Haskell插件。

> 译注: 现在Haskell已经支持语言服务协议(Language Server Protocol),你可以安装Haskell语言服务器(HLS),在你的编辑器中安装LSP插件,即可享受语法高亮,类型检查,语法提示等功能.朔曦推荐VSCode及其Haskell插件.
## Glasgow Haskell 编译器

Haskell是编译型语言，而Glosgow Haskell编译器实现了Haskell的强大潜能。编译器的工作是将人类可读的源代码转换为机器可读的二进制文件。在编译结束时，你会得到一个可执行的二进制文件。这与运行Ruby时不同，例如，在Ruby中，另一个程序(即 _解释器_ )读取源代码并动态解释运行。编译器相对于解释器的主要好处是，因为编译器会预先转换代码，它可以对你编写的代码进行分析和优化。由于Haskell的其他一些设计特性------即强大的类型系统------GHC能保证你的程序如这个谚语所说， _编译即正确_ 。虽然你会经常使用GHC，但不要认为这一切理所当然。它本身就是令人惊叹的软件。

调用GHC时,打开终端,输入`ghc` :
```
$ ghc
```

在本文中，只要遇到 `$` 符号，就意味着你在命令提示符中输入了内容。当然，如果没有文件要编译，GHC就会报错。首先，创建一个名为hello.hs的简单文件。在你选择的文本编辑器中，创建一个名为hello.hs的新文件。并输入以下代码。

<!-- \hslisting{hello.hs , 程序的第一声啼哭} -->
```haskell
--hello.hs 第一篇Haskell源代码,双横杠表示注释
main = do --程序的main函数
  print "Hello World!" --这个函数打印"Hello,World!"
```
目前不要太担心本节中的任何代码具体对应着发生什么。您的真正目标是学习所需的工具，以免后续学习Haskell要用工具时才发现生疏。

现在你有了一个示例文件，可以再次运行GHC，这次传入hello.hs 文件作为参数:
```
$ ghc hello.hs
[1 of 1] Compiling Main
Linking hello ...
```
如果编译成功，GHC将创建三个文件:

- hello (Windows上是hello.exe)
- hello.hi
- hello.o
首先，最重要的文件是hello，这是你的二进制可执行文件。因为这个文件是二进制可执行文件，你可以直接运行这它:
```
$ ./hello
"Hello World!"
```

请注意，编译后程序的默认行为是执行`main`中的逻辑。默认情况下，你编译的所有Haskell程序都需要有一个`main`函数，它的作用类似于Java/ C++ / C#中的`Main`方法或Python中的`__main__`。

像大多数命令行工具一样，GHC支持很多选项。例如，如果你想编译hello.hs,并将输出命名为 helloworld，你可以使用`-o`参数:

```
$ghc hello.hs -o helloword
[1 of 1] Compiling Main
Linking helloworld ....
```
更多编译器选项请查看`ghc --help`(不需要其他参数).

> 小测: 编译hello.hs输出另一个名为testprogram的文件.
> <details><summary>答案</summary> 
> 将程序复制到文件,然后在同目录运行`ghc hello.hs -o testprogram`
> </details>

## 与Haskell交互------GHCi

编写Haskell程序最常用的工具之一是GHCi，一个用于GHC的交互式接口。和GHC一样，GHCi也以一个简单的命令开始:`GHCi`。启动GHCi后，你会看到一个新的提示符:
```
$ ghci
GHCi> 
```

本书中`GHCi>`表示该行在GHCi内输入;由GHCi输出的行以空白开头。从命令行开始学习任何程序时，首先要了解如何结束它!对于GHCi，你可以使用`:q`命令来退出:
```
$ ghci
GHCi> :q
Leaving GHCi.
```


使用GHCi很像使用大多数解释型语言(如Python和Ruby)中的解释器。它可以用作一个简单的计算器:
```
GHCi> 1 + 1
2
```

你也可以在GHCi中动态编写代码:
```
GHCi> x = 2 + 2
GHCi> x
4
```
在GHCi版本8之前，函数和变量定义需要以let关键字开头。现在已经不需要这么做了，但网上和很多旧Haskell书中仍然有这个示例:
```
GHCi> let f x = x + x
GHCi> f 2
4
```

GHCi最重要的用途是与你正在编写的程序交互。有两种方法可以将现有文件加载进GHCi。一种是将文件名作为参数传递给ghci:

```
$ ghci hello.hs
[1 of 1] Compiling Main
Ok, modules loaded: Main.
```
另一种方法是在交互状态使用 `:l` (或 `:load`) 命令:
```
$ ghci
GHCi> :l hello.hs
[1 of 1] Compiling Main
Ok, modules loaded: Main.
```
 不论哪种情况,你都可以调用写好的函数:
```
GHCi> :l hello.hs
GHCi> main
"Hello World!"
```


不同于在GHC的编译文件，你的文件不需要`main`函数就可以加载到GHCi中。载入文件时，已有的函数和变量定义会被覆盖。您可以在编写和更改文件不断加载它。Haskell的独特之处在于既具有强大的编译器支持,又有自然且易于使用的交互环境。如果你使用的是Python、Ruby或JavaScript等解释型语言，那么使用GHCi就会感觉非常自在。如果您熟悉Java、C#或c++等编译语言，那么在编写Haskell时，您可能会惊讶于你使用的Haskell是编译语言.

> 小测: 编辑你的Hello World脚本使其输出Hello &lt;Name&gt;,其中&lt;Name&gt;是你的名字。在GHCi中重新加载这个脚本.
> <details><summary>答案</summary>
> 如此更改文件:
> <pre><code class="language-haskell">main = do 
>   print "Hello Will!"
> </code></pre>
> 在GHCi中加载文件:
> <pre><code>GHCi> :l hello.hs
> GHCi> main
> Hello Will!
> </code></pre>
> </details>

## Haskell 工作流

对于Haskell新手来说，首当其冲的问题是，Haskell的基本I/O反而是相当高级的话题。你接触一门语言时，往往是一边打印输出,一边理解程序的工作原理.可是在Haskell中，这样的临时调试通常不可能。在Haskell程序中很容易出现bug，伴随一堆相当复杂的错误，你很可能绷住,完全不知道如何继续。

好巧不巧,Haskell _出色的_ 编译器会对代码的正确性斤斤计较。如果你习惯于编写一个程序，运行它，并快速地修复你犯的任何错误，你就会收到一些小小的haskell震撼了。Haskell强烈鼓励在运行程序之前先把问题想清楚。等到你积累了足够的经验，我相信曾经挫折你的东西会成为你最喜欢的语言特性。在编译过程中磨砺正确性的另一面是，程序会正确工作，并且正确会成为日常,而不需要多次调试出来。

有个办法可以比较无痛地编写Haskell代码: 每次写一小点,交互式地运行它以验证结果。为了演示这一点，我们将选取一个混乱的Haskell程序，并一片一片地整理它，直到其易于理解。在这个示例中，你将编写一个命令行应用程序，起草作者发给读者的感谢信。下面是这个程序的第一个版本,写得很糟糕:

> first_prog.hs 的糟糕版本
```haskell
messyMain :: IO()
messyMain = do
  print "Who is the email for?"
  recipient <- getLine
  print "What is the Title?"
  title <- getLine
  print "Who is the Author?"
  author <- getLine
  print ("Dear " ++ recipient ++ ",\n" ++
    "Thanks for buying " ++ title  ++ "\nthanks,\n" ++
    author )
```

问题主要是这些代码全都混在这个大函数`messyMain`中。编写模块化代码在软件开发中普遍是良好实践, Haskell更进一步，要求你的代码便于理解和排查. 这个程序尽管乱，还是能跑。如果把`messyMain`的名字改成`main`，它就能编译运行了。你可以直接将这些代码加载到GHCi中，假设你和first_prog.hs在同一个目录下:
```
$ghci
GHCi> :l first_prog.hs
[1 of 1] Compiling Main     ( first_prog.hs, interpreted )
Ok, modules loaded: Main.
```
如果你得到了GHCi的`Ok`，你就知道你的代码已经编译并能正常工作了!注意，GHCi并不关心你是否有一个主函数。因为你还要与没有`main`的文件进行交互。现在可以测试你的代码了:

```
GHCi> messyMain
"Who is the email for?"
Happy Reader
"What is the Title?"
Learn Haskell
"Who is the Author?"
Will Kurt
"Dear Happy Reader,\nThanks for buying Learn Haskell\nthanks,\nWill Kurt"
```

一切正常，但这段代码如果稍微分解一下，就会好看得多。你的主要目标是创建一封电子邮件，但很容易看出电子邮件由三个部分组成:抬头、正文和签名。首先将生成这些部分的函数提取出来。下面的代码在first_prog.hs写入,本书中定义的几乎所有函数和值都可以假设写入到你正在使用的文件中。我们将从`toPart`函数开始:
```haskell
toPart recipient = "Dear" ++ recipient ++ ",\n"
```

在这个例子中，你可以很容易一口气写出这三个函数，但通常你值得放慢脚步，每写完一个就测试一个。为了测试这个函数，再次在GHCi中加载你的文件:

```
GHCi> :l "first_prog.hs"
[1 of 1] Compiling Main  ( first_prog.hs, interpreted )
Ok, modules loaded: Main.
GHCi> toPart "Happy Reader"
"DearHappy Reader,\n"
GHCi> toPart "Bob Smith"
"DearBob Smith,\n"
```
在编辑器中编写代码，然后一遍一遍将其加载进GHCi，这种使用代码的模式将贯穿全书。为避免重复，我们假定使用`:l "first_prog.hs"` ,不再特别写出。现在你已经将它加载到GHCi了，你会看到有一个小错误，Dear和收件人的名字之间缺少一个空格。让我们看看如何解决这个问题。
```haskell
-- 更正后
 toPart recipient = "Dear " ++ recipient ++ ",\n"
```
回到 GHCi:

```
GHCi> toPart "Jane Doe"
"Dear Jane Doe,\n"
```
好极了。现在来定义另外两个函数。这次就一口气写出来吧。我仍然要说,在以后的学习中，一次写一个函数，将其加载到GHCi中，保证它正确,再去写下一个,稳一些总没有坏处.

```haskell
bodyPart bookTitle = "Thanks for buying " ++ bookTitle ++ ".\n"
fromPart author = "Thanks,\n"++author
```
测试方法同理:

```
GHCi> bodyPart "Learn Haskell"
"Thanks for buying Learn Haskell.\n"
GHCi> fromPart "Will Kurt"
"Thanks,\nWill Kurt"
```
一切正常.现在让我们把所有元素综合在一起:
```haskell
createEmail recipient bookTitle author = toPart recipient ++
                                         bodyPart bookTitle ++
                                         fromPart author
```

注意这三个函数调用的对齐方式。Haskell确实用缩进表示语义(但不像Python那样强调)。假定本文中的任何格式都是有意为之;如果代码段对齐，那肯定是有原因的。大多数编辑器都可以通过Haskell插件自动处理缩进。

写好所有函数后，就可以测试`createEmail`了:

```haskell
GHCi> createEmail "Happy Reader" "Learn Haskell" "Will Kurt"
"Dear Happy Reader,\nThanks for buying Learn Haskell.\nThanks,\nWill Kurt"
```
你的函数正常运行.现在在`main`中把它们组合起来:
```haskell
-- first_prog.hs 的main,现在好看些了
main = do
  print "Who is the email for?"
  recipient <- getLine
  print "What is the Title?"
  title <- getLine
  print "Who is the Author?"
  author <- getLine
  print (createEmail recipient title author)
```

你应该准备好编译了,当然你可以在 GHCi 中测试函数,不过这不是必须的:

```
GHCi> main
"Who is the email for?"
 Happy Reader
"What is the Title?"
Learn Haskell
"Who is the Author?"
Will Kurt
"Dear Happy Reader,\nThanks for buying Learn Haskell.\nThanks,\nWill Kurt"
```

既然你把具体的组件组装起来了，并且你能够单独测试它们的行为,保证马哥组件都正常工作。那么，你可以编译了:

```
$ ghc first_prog.hs 
[1 of 1] Compiling Main     ( first_prog.hs, first_prog.o )
Linking first_prog ...
$ ./first_prog 
"Who is the email for?"
Happy Reader
"What is the Title?"
Learn Haskell
"Who is the Author?"
Will Kurt
"Dear Happy Reader,\nThanks for buying Learn Haskell.\nThanks,\nWill Kurt"
```

这是你编写的第一个有工作意义的Haskell程序。了解了基本的工作流程后，您现在可以深入Haskell的神奇世界了!

> 译注: 在上面的程序中,你有没有注意到,每次你获取一个项时,先打印一个提示语句,然后获取用户输入,这个过程本身就可以抽象出来.如果你了解python 的`input`函数,就会知道它的一个参数就是提示语句.由此,我们可以进一步化简这个程序:
> ```haskell
> ask :: String -> IO String
> ask prompt = putStrLn prompt >> getLine
> ```
> 然后这个`main`还能进一步化简:
> ```haskell
> main :: IO ()
> main = do   -- 欢迎来到抽象的 Haskell !
>   recipient <- ask "Who is the email for?"
>   title     <- ask "What is the Title?"
>   author    <- ask "Who is the Author?"  
>   print (createEmail recipient title author)
> ```
> 至于这个`ask`函数的类型是什么意思, 它为什么能像`getLine`一样用`<-`取值, `putStrLn` 和 `getLine` 中间的 `>>` 又是什么? 前面的区域以后再来探索吧.

## 总结

在这一课中,你的目标就是迈出使用Haskell的第一步。你首先安装了Haskell的工具,包括编译器GHC, 交互式解释器GHCi,以及本书后面要用的构建工具stack。本课的其余部分涵盖了编写、重构、运行和编译Haskell程序交互和编译的基础知识。现在来做两个练习吧:

**Q1.1** 在GHCi中求2<sup>123</sup>.

**Q1.2** 修改first_prog.hs中每个函数的文本,同时在GHCi中测试它们，最后，编译一个新版本的电子邮件模板程序，使可执行文件命名为email。


