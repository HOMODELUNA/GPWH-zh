# Haskell 上手
学习了第1课,你将能够:

- 为 Haskell 开发安装好工具
-  使用 GHC 和 GHCi
-  Use tips for writing Haskell programs

## 欢迎来到 Haskell!

在深入学习Haskell之前，您需要熟悉您将在旅程中使用的基本工具。本课将引导您接触Haskell。这一课从下载编写、编译和运行Haskell程序的基础知识开始。然后，请阅读示例代码，并思考如何用Haskell编写代码。千里之行,始于足下.

### Haskell Platform
学习一门新的编程语言最糟糕的部分是第一次搭建开发环境。幸运，甚至有些惊讶的是，这在Haskell中根本不是问题。Haskell社区已经整合了一个单一的、开箱即用的工具包，称为Haskell Platform。\footnote{ }

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

