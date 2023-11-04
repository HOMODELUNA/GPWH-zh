# 一等函数

学习了第4课,你将能够:

- 理解一等函数的定义
- 将函数用作其他函数的参数
- 从函数中抽象出计算流程
- 将函数作为值返回


虽然函数式编程长期以来都被认为过于学术化声，但函数式编程语言的几乎所有关键特性都开始出现在许多其他更主流的编程语言中。其中最广泛流传的特性是一等函数,即可以像值一样被传递和返回的函数。十年前，这个想法让许多程序员感到震惊，但今天大多数编程语言都支持并经常使用这个概念。如果您曾经在JavaScript中指定事件回调函数或给Python等语言中的sort方法传递自定义的排序逻辑，那么您已经使用过一等函数了。

> **考虑这个** 你想创建一个网站，对一件商品比较亚马逊(Amazon)和eBay等其他网站上同类商品的价格。你已经有了一个返回所需商品URL的函数，但还需要为每个网站编写代码，以从页面中提取价格。一种解决方案是为每个站点创建一个自定义函数:
> ```haskell
> getAmazonPrice url
> getEbayPrice url
> getWalmartPrice url
> ```
> 这没什么问题，只是这些函数有很多相同的逻辑.例如，将字符串价格(如$1,999.99 )解析为数值类型(如1999.99)。这个函数能不能被进一步细分,使一个函数从HTML中提取字符串,另一个函数将字符串转成数字? 如果这样,我们就可以写一个通用的`getPrice`函数,并将这些函数作为参数传递进去.

## 作为参数的函数

一等函数的理念是，函数与程序中使用的任何其他数据没有什么不同。函数可以用作参数，也可以作为其他函数的返回值。对于编程语言来说，这看上去是一个强大的功能。它允许你从代码中抽象出任何重复的计算，并最终允许你用函数来编写其他函数。

假设你有一个函数`ifEveninc`，它会在数字为偶数时增加n;否则，它会原原本本地返回数字，如下面的代码所示。

```haskell
ifEvenInc n = if even n
              then n + 1
              else n
```
稍后你会发现还需要两个函数:`ifEvenDouble`和`ifEvenSquare`，它们分别对偶数乘二或求平方，如下所示。如果你知道如何编写`ifEveninc`，这些函数都很容易编写。

```haskell
ifEvenDouble n = if even n
                 then n * 2
                 else n
ifEvenSquare n = if even n
                 then n^2 
                 else n
```
虽然这三个函数很容易编写，但几乎完全相同。唯一的区别在于递增、倍增和平方的行为。由此你发掘出一种可以抽象出来的通用计算模式。这个抽象模式进行了一些计算,然后它怎么知道如何进行接下来的计算呢?要做到这一点，关键在于能够将函数作为参数传递，以执行所需的行为。

以`ifEven`函数为例，它接受一个函数和一个数字作为参数。如果该数字是偶数，`ifEven`就对该数字应用一个函数。
```haskell
ifEven myFunction x = if even x
                      then myFunction x
                      else x
```
你还可以将递增、加倍和平方的行为抽象为三个独立的函数:

```haskell
inc n = n + 1
double n = n*2
square n = n^2
```
让我们看看如何借助一等函数来重新创建前面的定义:

```haskell
ifEvenInc n = ifEven inc n
ifEvenDouble n = ifEven double n
ifEvenSquare n = ifEven square n
```
现在你就能进一步扩展这个函数的行为了,比如创建`ifEvenCube`或 `ifEvenNegate`.

> **函数和运算符优先级**
> 这一课介绍过了函数和运算符的。例如，`inc`是一个函数，`+`是一个运算符。编写Haskell代码需要注意的一点是，**函数总是在运算符之前求值**。这是什么意思?看看GHCi中的这个例子:
> ```
> GHCi> 1 + 2 * 3
> 7
> ```
> 与大多数编程语言一样，`*`的优先级高于`+`，因此将2乘以3，再加1，得到7。现在让我们看看将`1 +`替换为`inc`时会发生什么:
> ```
> GHCi> inc 2 * 3
> 9
> ```
> 结果之所以不同，是因为函数的优先级总是高于运算符。这意味着首先计算`inc 2`，然后将结果乘以3。即使对于多参数函数也是如此:
> ```
> GHCi> add x y = x + y
> GHCi> add 1 2 * 3
> 9
> ```
> 这么设定的主要好处在于你可以省下很多不必要的括号.

### Lambda函数作为参数
一般来说，给函数命名是个好主意，但你也可以使用lambda函数来快速添加代码并传递给函数。如果你想将值加倍，编写一个lambda函数来的更快:

```
GHCi> ifEven (\x -> x*2) 6
12
```

虽然工程上推荐给函数命名,但很多时候你传入简单的功能就不必特地写函数了。

> 译注: Haskell 有一个语法糖,叫做"部分表达式",主要的含义是:一个被空出一段的表达式会被理解成一个lambda函数:
> ```haskell
> (* 2) --理解成 (\x -> x*2)
> (2 *) --理解成 (\x -> 2*x),注意参数被应用的位置 
> ```
> 上面的那个例子在实践中往往写成`ifEven (*2) 6`,这是一种约定俗成的写法.
>
> 另外一点,函数的部分应用也可以成为部分表达式,但是参数的应用顺序有区别.如 `(+)` 本身就可以当成一个二元函数`\left right -> left + right`,所以`((+) 2)`中,2被填进了第一个参数中,而不是第二个,这个函数应当理解为`\right -> 2 + right`.这个例子还不明显,你换成除法函数`/`就知道问题了.
>

> 小测: 写一个lambda函数,将输入x变为其立方,而后将其传递给`ifEven`
> <details><summary>答案</summary>
> <code>\x -> x ^ 3</code>,然后<code>ifEven (\x -> x ^ 3) 4</code>

### 例子: 自定义排序

将函数传递给其他函数的一个实际应用是排序。假设你有一个姓和名的列表。在这个例子中，每个名字都表示为一个元组。元组是一种类似于列表的类型，但它可以包含多种类型，不过大小是固定的。下面是用元组表示名称的例子:
```haskell
author = ("Will","Kurt")
```
包含两个元素的元组(一对)有两个有用的函数`fst`和`snd`，它们分别访问元组的第一个和第二个元素:

```
GHCi> fst author
"Will"
GHCi> snd author
"Kurt"
```
现在假设你要排序一个名字列表。这里是一列以元组表示的名称.

```haskell
names = [("Ian", "Curtis"),
         ("Bernard","Sumner"),
         ("Peter", "Hook"),
         ("Stephen","Morris")]
```
你想对名字排序。很幸运，Haskell确实有内置的排序函数。要使用它，首先需要导入模块`Data.List`。这到简单，你需要将以下声明添加到当前文件的顶部:
```haskell
import Data.List
```

或者，您可以将`Data.List`导入到GHCi。如果你加载一个带`names`和`import`声明的文件，就可以看到Haskell的`sort`猜测了如何对这些元组排序:
```haskell
GHCi> sort names
[("Bernard","Sumner"),("Ian", "Curtis"),("Peter", "Hook"),("Stephen","Morris")]
```
还不错，即使Haskell根本不知道你想干什么!不幸的是，你通常不想按名字排序。要解决这个问题，可以使用Haskell的`sortBy`函数，它就包含在模块`Data.List`中。你需要为`sortBy`提供另一个函数，用来比较两个名称元组。在解释了如何比较两个元素之后，剩下的就交给`sort`函数了。为此，需要编写函数`compareLastNames`。这个函数接受两个参数，`name1`和`name2`，并返回`GT`、`LT`或`EQ`。`GT`、`LT`和`EQ`是特殊值，表示大于、小于和等于。在许多编程语言中，你会返回`True`或`False`之一、或是1、-1或0的三者之一。

```haskell
compareLastNames name1 name2 = 
  if lastName1 > lastName2
  then GT
  else if lastName1 < lastName2
       then LT
       else EQ
  where lastName1 = snd name1
  lastName2 = snd name2
```

现在你可以回到GHCi并使用sortBy进行自定义排序:

```
GHCi> sortBy compareLastNames names
[("Ian", "Curtis"),("Peter", "Hook"),("Stephen","Morris),("Bernard","Sumner")]
```

好多了!JavaScript、Ruby和Python都支持使用类似的一等函数进行自定义排序，因此许多程序员可能对这种技术很熟悉。

> 小测: 在`compareLastNames`中，我们没有处理两个姓相同但名不同的情况。修改`compareLastNames`函数，使其在姓相同的时候,比较名,将其结果作为整体的输出。
><details><summary>答案</summary><pre><code class='language-haskell'>compareLastNames name1 name2 = 
>   if lastName1 > lastName2
>   then GT
>   else if lastName1 < lastName2
>        then LT
>        else if firstName1 > firstName2
>             then GT
>             else if firstName1 < firstName2
>                  then LT
>                  else EQ
>     where lastName1 = snd name1
>           lastName2 = snd name2
>           firstName1 = fst name1
>           firstName2 = fst name2
> </code></pre>
> 译注:等学了模式匹配后,我们有比较简单的写法,现在这个if-else长链太难看了.
> <pre><code class='language-haskell'>compareLastNames (fn1,ln1) (fn2,ln2) = 
>   mappend (compare ln1 ln2) 
>           (compare fn1 fn2)</code></pre>
> <code>compare</code> 就是比较两个值,得出<code>GT</code>,<code>EQ</code>,<code>LT</code>之一,而<code>mappend</code>在此处的意思是:先看第一个值,如果第一个是<code>EQ</code>,那么就看下一个,否则结果就是第一个值.
> </details>
> 

## 函数作为返回值

我们已经讨论了很多关于将函数作为参数传递的内容，但这仅仅是将函数作为一等公民意义的一半。函数也有返回值，因此对于真正的一等函数来说，函数有时必须返回其他函数。和往常一样，问题应该是，为什么我要返回一个函数?一个很好的理由是，你希望根据其他参数分发某些函数。

假设你创建了一个当代炼金术士的秘密社团，你需要向位于不同地区邮局的成员发送时事通讯。公司在三个城市设有办事处:旧金山、里诺和纽约。以下是办公室地址:

- PO Box 1234, San Francisco, CA, 94111
- PO Box 789, New York, NY, 10013
- PO Box 456, Reno, NV, 89523

你需要构建一个函数，它接受一个名称元组(就像在排序示例中使用的那样)和一个办公地点，然后组合出一个邮件地址。这个函数的第一个版本可能如下所示。还有一件你还没学到的事情需要介绍，那就是用于连接字符串(和列表)的`++`操作符。

```haskell
addressLetter name location = 
  nameText ++ " - " ++location
    where nameText = (fst name) ++ " " ++ (snd name)
```

要使用这个函数，你需要传递一个名称元组和完整的地址:

```
GHCi> addressLetter ("Bob","Smith") "PO Box 1234 - San Francisco, CA, 94111"
"Bob Smith - PO Box 1234 - San Francisco, CA, 94111"
```
是个办法。您还可以使用变量来记录地址，这将大大减少错误(并节省输入)。你已经准备好发送你的时事通讯了!

在第一轮简报之后，你会收到一些来自区域办事处的抱怨和要求:

- 旧金山为姓氏以字母L开头或更晚的成员添加了一个新地址:PO Box 1010, San Francisco, CA, 94109。

- 纽约希望在名字后面加上冒号而不是连字符，因为一些神秘的原因。

- 雷诺只希望使用姓氏来增强保密性。

很明显，现在需要为每个办公室设置不同的函数,以满足其需求。

```haskell
sfOffice name = if lastName < "L"
    then nameText 
      ++ " - PO Box 1234 - San Francisco, CA, 94111"
    else nameText 
      ++ " - PO Box 1010 - San Francisco, CA, 94109"
  where lastName = snd name
        nameText = (fst name) ++ " " ++ lastName

nyOffice name = nameText ++ ": PO Box 789 - New York, NY, 10013"
  where nameText = (fst name) ++ " " ++ (snd name)
renoOffice name = nameText ++ " - PO Box 456 - Reno, NV 89523"
  where nameText = snd name
```
现在的问题是，如何在`addresletter`中使用这三个函数?你可以重写`addresletter`，让它接受一个函数而不是位置作为参数。这样做的问题是，`addresletter`函数将是一个更大的web应用程序的一部分，而你想传递一个字符串参数作为位置。你真正想要的是另一个函数，它将接受一个`location`字符串，并为你分发正确的函数。你将构建一个名为`getLocationFunction`的新函数，它接受一个字符串并分发正确的函数。我们将使用Haskell的`case`表达式，而不是一堆嵌套的`if then else`表达式。

```haskell
getLocationFunction location = 
  case location of --看看location的值是什么
    "ny" -> nyOffice --如果值是"ny",返回nyOffice
    "sf" -> sfOffice --如果值是"sf",返回sfOffice
    "reno" -> renoOffice --如果值是"reno",返回renoOffice
    --如果都不是 ( _ 是通配符)
    _ -> (\name -> (fst name) ++ " " ++ (snd name))
    --则返回通用解决方案
```

这个`case`表达式看着很直白，除了末尾的下划线(`_`)。即使传来的不是正式的位置,你仍然希望你对它有办法处理。在Haskell中，\_ 经常用作通配符。(下一课中会更深入地讨论)。在这种情况下，如果代码使用者传递了一个无效的位置，你可以编写一个快速的lambda函数，将`name`元组转换为字符串,以此处理这个"不意的情况"。现在你有了一个函数，根据你输入的地点名，它会返回你需要的函数。最后，重写`addresletter`，如下所示。

```haskell
addressLetter name location = locationFunction name
  where locationFunction = getLocationFunction location
```
在 GHCi 中确认函数行为是否符合预期:
```
GHCi> addressLetter ("Bob","Smith") "ny"
"Bob Smith: PO Box 789 - New York, NY, 10013"
GHCi> addressLetter ("Bob","Jones") "ny"
"Bob Jones: PO Box 789 - New York, NY, 10013"
GHCi> addressLetter ("Samantha","Smith") "sf"
"Samantha Smith - PO Box 1010 - San Francisco, CA, 94109"
GHCi> addressLetter ("Bob","Smith") "reno"
"Smith - PO Box 456 - Reno, NV 89523"
GHCi> addressLetter ("Bob","Smith") "la"
"Bob Smith"
```

现在您已经分离了生成地址所需的每个函数，即使别的办公室又提出了新的要求,您也可以轻松地扩展。这是将函数作为值返回的一种简单用法;你所做的只是扩展了函数的组合方式。

## 总结
这一课的目标是讲解一等函数。一等函数允许你将函数作为参数传入，也允许你将函数作为值返回。它允许您从函数中抽象出计算来。这一性质因为其功能强大,在大多数现代编程语言中得到广泛应用。然后,来做一做习题吧。


**Q4.1** Haskell给任何可以进行比较的东西(例如，`[Char]`，你用它来表示名字元组中的名字)都配备了`compare`函数。它接受两个要比较的值,返回`GT`、`LT`或`EQ`其中之一。使用`compare`重写`compareLastNames`。

**Q4.2** 为Washington DC的办公室定义一个新的位置函数，并将其添加到`getLocationFunction`中。在DC函数中，每个人的名字后面都必须加上`Esq`。


