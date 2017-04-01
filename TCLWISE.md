[TOC]

# 介绍

 亲爱的读者，这本书是对Tcl编程语言的主要思想的介绍：如果你想学习一种简单而强大的编程语言，这本书是为你而设。 要阅读这本书，所需的唯一先前的知识是对任何语言的编程的一些基本的了解：从C到Python，Perl，Lisp，如果你明白功能和变量的含义以及其他基本概念，你应该不会在接下来的阅读中遇到问题。 

为什么选择Tcl？ 因为它是一种简单而通用的编程语言，可以在短时间内成功开发应用程序。  您接下来将发现Tcl是一种可编程的编程语言。 TCL包含了几个可以用来组合创建程序的理念，可以扩展语言本身以便以非常直接的方式来解决编程问题。

>   Tcl由John Ousterhout在1988年左右创建为可嵌入的命令语言。 今天，Ousterhout不再积极开发语言，Tcl的演变掌握在Tcl核心团队以及Tcl的社区手中，该团队在2000年夏天指导被选为Tcl Core的发展。  Tcl核心团队正在致力于Tcl 8.5版本，该版本应该在2005年秋季发布。 

# 基础

## 命令解剖

Tcl程序由命令组成，因此命名为Tcl（发音为Tickle）：Tool Command Language(工具命令语言)。 一个命令是一系列由空格分隔的单词，如下例所示：

```Tcl
puts Hello
```

第一个单词是Tcl过程的名称（或多或少与C中的*函数*相同），其后面的单词是参数。 在这个例子中只有“Hello”参数。 

 上面的命令告诉Tcl用参数“Hello”来调用**puts**过程。 由于使用**puts**过程将参数输出到终端，所以该命令的效果是写“Hello”。 

 如果你想尝试这个命令（我强烈建议你用这本书中的例子），运行*tclsh* ，即Tcl shell，一个程序，你可以在其中键入Tcl命令，并在屏幕上交互地看到结果，然后输入：“puts Hello”，后跟<Enter>键。  Tcl将执行该命令并输出“Hello”到终端： 

```Tcl
% puts Hello
Hello
% 
```

Tcl中包含一套标准的程序，如**puts** ，但用户可以定义自己的程序。 与许多其他语言不同，与Tcl默认的过程和用户定义的过程之间的程序员观点没有真正的区别，我们稍后会看到更好。 我们将参考Tcl中包含的默认程序作为*核心程序* 。 实际上，Tcl用户经常调用过程*命令* ，即使命令实际上是过程名称和参数的总和。 但是由于这是用过的术语，我们还将把*核心程序*称为*核心命令* ，一般来说可以用可交换的方式使用命令和过程。 

 设置一个重要的核心命令，它有两个参数：变量的名称和一个字符串。  **Set**将字符串分配给变量，并返回分配的值。 每个Tcl命令返回一个值，但返回值根本不重要的一些命令返回一个空字符串，如**puts** ，因为我们对调用**puts**的效果感兴趣，而不是返回的值。 

这是**set**命令的使用示例： 

```Tcl
% set a apple
apple
%
```

如果我们指定一个不存在的变量，结果就是一个错误：

```Tcl
% set b
can't read "b": no such variable
while evaluating {set b}
```

在Tcl变量中没有默认值。 如果您尝试使用从未定义的变量，则结果是一个错误。 

## 分组

我们可以编写一个打印“Hello World！”的程序吗？ 肯定的，但还有更多的东西要学习，叫做分组。 为了使**put**命令工作，程序员需要传递一个字符串作为一个参数。 如果我们写：

```Tcl
puts Hello World!
```

实际上用**两个**参数被命令调用。 第一个是“Hello”和第二个“World！”，所以结果是一个错误。 要在单个参数中分组包含空格的字符串，请使用引号：

```Tcl
% puts "Hello World!"
Hello World!
```

 这样程序的工作完美。 还有另一种引用方式，正如我们将在下一节中讨论的*替换* 一样。 

## 方案结构

还有一些关于Tcl程序基本结构的信息。 首先，程序中的不同命令被划分为换行符，所以你可以编辑一个文本文件，并编写这个程序：

```Tcl
puts "Hello World"
puts "Ciao Mondo"
```

保存为hello.tcl，并执行它。 如果你是一个unix用户，你可以从unix shell中调用它：

```Tcl
tclsh hello.tcl
```

并看到输出。 另外要知道的是，命令也可以用“;”分隔，以便在单行中写入更多的命令。 所以以下程序完全等同于之前的程序：

```Tcl
puts "Hello World"; puts "Ciao Mondo"
```

在接下来的章节中，我们将编写更复杂的程序，这些程序在*tclsh*中直接输入是不舒服的，因此您可能希望将代码键入文件，并运行将文件名作为参数传递给*tclsh的程序* 。

## 替换命令

 您可能需要使用**puts**命令才能在屏幕上打印变量的值。 为了做到这一点，我们需要一种方法来使用变量的值作为**puts**命令的参数。 这是怎么做到的： 

```Tcl
% puts [set a]
apple
%
```

 这叫做*命令替代* 。 简而言之，Tcl命令可以包含嵌套在[和]括号内的其他命令：Tcl将首先计算此命令的结果，替换此结果代替[和]（括号括起来）之间的所有内容，最后调用**puts**命令。 这就是Tcl解释器将如何一步一步处理命令。 

Tcl尝试处理命令：

```Tcl
puts [set a]
```

它将用[set a]替换它返回的值来获取：

```Tcl
puts apple
```

最后会调用**puts**命令*，就像直接用“apple”作为参数*一样调用。 替代的命令可能又包含更多的命令来替代。 当然，许多参数或命令中的一部分参数可能是命令来替代。 看到这个例子，通常直接输入到*tclsh*中：

```Tcl
% set a apple
apple
% set b orange
orange
% puts "I want an [set a] and an [set b]"
I want an apple and an orange
```

 这两个命令都是从左到右替换的。 正如您可以看到命令替换工作在引号内，实际上在示例中有一个参数中有两个命令替换，它们与其他字符串混合。 这称为*插值* 。 

## 替换变量

您可能会认为，如果您只需要像以前的示例中那样替换一个变量，则命令替换有点冗长，很难输入。 这就是为什么Tcl也支持将变量替换为特殊情况。 所以为了写[set a]，你可以在下面的例子中写一个$ a（假设变量*a*和*b*已经被设置为一个值）：

```Tcl
% puts "I want an $a and an $b"
I want an apple and an orange
```

正如你可以看到，Tcl插补程序不限于变量，而是限于完整的命令。 这和我们稍后会探讨的其他功能使得Tcl在使用字符串时非常方便，而今天许多东西都是字符串：从xml，html到许多网络协议，配置文件等。 

 现在，命令和变量替代概念应该是清楚的，我们可能想知道如何能**不**替代。 你还记得引号用于分组，但是如预期的那样，还有另一种形式的分组，使用{和}代替引号。 它工作相同，但不允许命令和变量替换： 

```Tcl
% puts {I want $a and $b}
I want $a and $b
```

  “$ a”和“$ b”现在逐字地打印，不用任何方式处理。 

## 关于插值的更多内容

重要的是要意识到插补是**在**隔离参数**之后**完成的，例如写：

```Tcl
set a "puts Hello"
$a
```

将不会工作，Tcl将尝试调用一个名为“puts Hello”的命令。 相反，以下将工作：

```Tcl
set a "pu"
set b "ts"
$a$b Hello
```

 上面的脚本将在屏幕上打印Hello，因为Tcl将扩展$ a和$ b，在同一个参数中连接，获得“puts”，但是您可以看到$ a $ b在插值阶段之前已经是一个唯一的参数。 

## 注释

 像许多不同的语言一样，您可以在Tcl代码中写出由解释器跳过的注释：它们对于人类来说很有用，以便更好地了解程序的某些部分，这些程序可能不太简单，只需要查看代码。 在Tcl中，注释以**＃**字符开头，并在遇到换行符时结束： 

```Tcl
# 这是一个注释，下一行会打印你好
puts "Hello"
```

注释可以从Tcl命令的预期位置开始，因此可以在同一行中显示一行代码：

```Tcl
puts "Hello" ; # 这是一个注释，下一行会打印你好
```

 要谨慎使用注释，尽量避免注释琐事，如： 

```Tcl
set a 5; # 将5设置为a的值
```

从代码上看就不言而喻的含义是不需要注释的。

## 结语

不管你信不信，你几乎会用Tcl了。 当然，要成为一个有经验的Tcl程序员还有许多其他的东西要学，但Tcl的主要思想已经揭示了：命令，分组和替换命令和变量的概念。 

 下一步是学习一些更多的命令，以便能够编写更多有趣的程序。 命令在Tcl中是中心的，即使是命令也是条件，这个命令的每个参数都是字符串，这些字符串是由命令来解释的。 

#  一切都是一个字符串 

在Tcl中，每种数据由字符串表示：数字，列表，代码，字典。 用户不应该提供一个具有正确数据类型的命令：“2”与2或2的{2}相同，如果给定的过程以这种方式解释，则为2，或者可能只是一个用于另一个过程的字符串，所以命令的参数总是只是字符串，命令将以某种方式解释。 

 例如，要计算Tcl中的数学表达式，有一个名为**expr**的命令，它将一个数学表达式作为参数，并返回该表达式的值。 

```Tcl
% expr 20+30
50
%
```

但我也可以写这段代码：

```Tcl
% set a "2"; set b "0+30"; expr $a$b
50
% 
```

Expr也可以计算布尔表达式：

```Tcl
% set a 5; set b 2
2
% expr $a > $b
1
```

它返回1，因为5> 2.像在C中，expr的逻辑表达式将返回1为true，0为false。 

 你可能想知道为什么Tcl中的数学没有使用每个数学运算符的命令来完成，例如：+的命令，等等，等等，像这样： 

```Tcl
% + 1 2
3
```

 和这个： 

```Tcl
% + 1 [* 2 40]
81
```

只是为了方便用户。 就个人而言，我喜欢这种方式，但Tcl设计师介绍了**expr** ，因为大多数人在学校用中缀符号写数学表达式。 实际上对于复杂的数学表达式来说，这是非常舒服的。 仍然有一个建议将此命令添加到Tcl。 但是，我们当然可以在Tcl中定义自己的过程，所以我们来看看如何创建一个+命令。

## 用户定义的程序

 要创建一个过程，使用**proc**命令： 

```Tcl
% proc + {a b} {expr $a+$b}
% + 3 4
7
```

现在我们可以像任何其他Tcl过程一样使用+过程，但最好分析**proc**命令创建**过程**所需的参数。 第一个参数是我们要创建的程序的名称，“+”。 第二个是表示过程在输入中输入的参数的字符串，即a和b。 这些参数作为唯一字符串传递给**proc** ，其中每个元素都用空格分隔。 最后一个参数是表示每次调用+过程时执行的Tcl代码的字符串。 

 所以当我调用命令+时，代码“expr $ a + $ b”在一个上下文中执行，其中varialbes *a*和*b*具有传递给过程的参数的值。 返回值是表达式的结果，因为默认情况下，Tcl过程返回执行的最后一个命令返回的值。 

 有一件非常重要的事情要注意。  **proc**过程创建其他过程，但没有什么特别之处，它只是一个Tcl过程本身，它将字符串作为参数。 现在通过{和}分组的重要性应该是明确的。 如果我们写： 

```Tcl
proc + "a b" "expr $a+$b"
```

我们得到一个错误：

```Tcl
% proc + "a b" "expr $a+$b"
can't read "a": no such variable
while evaluating {proc + "a b" "expr $a+$b"}
```

Tcl解释器尝试在$ a和$ b之前扩展以调用proc命令。 请注意，可以使用反斜杠来引用$字符，所以相同的代码实际上可以写成：

```Tcl
% proc + "a b" "expr \$a+\$b"
% + 1 1
2
```

 正如你可以看到**proc**只是一个过程，并且{和}不会向在C中一样识别为一个代码块，而只是字符串分组。 全部是一个字符串。 你甚至可以重新定义proc命令，如果你愿意的话。 

```
% proc proc {a b} {puts "$a likes $b"}
% proc Bill Windows
Bill likes Windows
```

现在不要这样做;）即使这样，你会发现有趣的用法。 如果您用最后一个例子重新定义了**proc**命令，只需重新启动*tclsh* ，否则将无法创建新的过程。

## if命令

 让我们来看一个新的命令： **if** 它就像是用任何其他语言，但是又是一个需要字符串的命令。 以下是使用if的代码示例。 这个时候不要将代码输入到*tclsh中* ，而是在使用编辑器的文件中输入代码，并执行它将文件名传递给*tclsh* 。 

```Tcl
proc abs x {
    if {$x > 0} {
	  return $x
    } else {
	  expr -$x
    }
}


puts [abs 10]
puts [abs -13]
```

上面的代码创建一个**abs** proc，它返回一个数字的绝对值（也就是说，x如果x> 0，如果x <0，则为0-x）。 在这个例子中， **if**命令需要四个参数。 第一个是要测试的**表达式** ： **如果**测试这个表达式在内部调用**expr**过程，如果结果为true，则执行作为第二个参数传递的代码，否则执行作为最后一个参数传递的代码。 在**abs**程序中，我们首次使用**return**命令。 目的是放弃执行当前的过程，继续执行调用过程，并重试指定的值（`$ x`在上面的`return $ x`命令中）。

有一个有趣的细节要注意。 您已经知道，不同的命令是用新的行分隔的。 所以有效的是写：

```Tcl
if {$a > 4} {
    puts Hello!
}
```

 或者只是： 

```Tcl
if {$a > 4} {puts Hello!}
```

但我不能写这个：

```Tcl
if {$a > 4}
{
    puts Hello!
}
```

因为有**两个**命令。 第一个是**如果**有一个参数，第二个是`{ ... puts Hello! ... } `命令！ 但是，我们当然希望很好地缩进代码，为了做到这一点，只需使用以下格式即可：

```Tcl
if {$a > 4} {
	puts Hello!
}
```

因为第二个参数以{字符开头，所有其余参数将作为单个参数，直到遇到下一个参数为止。 分组不会被换行符停止。 所以实际上，使用**expr**和分组Tcl程序或多或少地出现在从语法语言角度编写的程序C中，而在某种程度上，它们应该与使用中缀表达式的其他语言（如Lisp）更相似。 

 但是，我们需要在**if**命令上多说一点，例如简单的形式：`if <expression> truebranch> else <falsebranch>`可以省略“else”。 

```Tcl
% if 1 {puts Hello!} {puts Ciao!}
```

它的工作方式与上一个参数之间有“else”一样。 要注意的第二件事是， **if**是一个命令，它返回一个值，准确的说它返回由最后一个命令返回的值，所以例如你可以写：

```Tcl
% set max [if {$a > $b} {expr $a} {expr $b}]
```

C程序员可能会注意到这与三元运算符类似。 在上面的例子中，expr仅用于确保两个分支的最后一个命令返回我们感兴趣的数值。

最后要知道（至少现在）关于if / elseif / elseif / else表单的语法如下：

```Tcl
if {$a > 0} {
  set x "It's positive"
} elseif {$a < 0} {
  set x "It's negative"
} else {
  set x "It's zero"
}
```

**if**过程不是每个Tcl解释器默认包含的唯一条件。 还有**switch**和其他流程控制命令，如**for**和**while** 。 此外，用户可以创建新的条件或循环命令（但这是更进一步章节的参数）。

# 列表

## Tcl列表

 在Tcl中，所有内容都表示为一个字符串。 列表不能逃避语言的这个基本规则，而是列表的字符串表示是非常符合直接的人类思考的。  Tcl列表的最简单形式是具有零个或多个空格分隔字的字符串。 例如，字符串`a b c`是一个三元素列表，您可以使用**llength**命令测试它，它将作为参数作为Tcl列表，并返回此列表中存在的元素数量： 

```Tcl
% llength "a b c"
3
```

空列表由空字符串表示，即“”或{}。

```Tcl
% llength {}
0
```

还有一个从名为**lindex**的列表中提取给定元素的命令： 

```Tcl
% lindex "this is a list" 1
is
```

您可以看到，列表索引是*基于零的* 。 第一个元素是索引0（在我们的示例中为“a”），第二个元素为索引1，依此类推。  **Lindex** ，通常每个与列表索引作为参数相关的列表相关命令也接受“end”作为索引，用于指定列表的最后一个元素。 还有`end- <number>`形式，表示列表的最后一个`<number>`元素。 一些例子：

```Tcl
% set mylist "foo bar biz! Tcl"
foo bar biz! Tcl
% lindex $mylist end
Tcl
% lindex $mylist end-1
biz!
```

 您可能会想知道如何创建一个元素包含空格的列表。 该解决方案已经实际显示，称为分组： 

```Tcl
% llength "a b {c d} e"
4
```

 但是当您需要编写复杂的列表时，要使用的命令是使用仅为此创建的命令，即**list**命令： 

```Tcl
% list a b "c d" e
a b {c d} e
```

 列表使用每个输入参数作为列表的元素创建一个列表。 我们可以用它创建各种列表，当然也可以嵌套： 

```Tcl
% set mylist [list a b [list 1 2 3 4] c d]
a b {1 2 3 4} c d
% lindex $mylist 2 2
3
```

 可以看到**lindex**命令可以直接访问嵌套列表。  “lindex $ mylist 2 2”表示访问$ mylist列表中第二个元素的第二个元素。 

## foreach命令

**Foreach**是一个非常强大的命令，用于为一个或多个列表的每个元素执行Tcl脚本。 命令*签名*在其最简单的用法是： 

```Tcl
foreach varname list script
```

以下是打印屏幕上前五个自然数的平方的示例。

```Tcl
set mylist {1 2 3 4 5}
foreach e $mylist {
    puts [expr $e*$e]
}
```

 对于列表的每个元素，foreach将其值分配给指定的变量并执行脚本。 实际上而不是单个变量，用户可以提供变量列表，如下例所示： 

```Tcl
set mylist {1 2 3 4 5 6}
foreach {x y} $mylist {
	puts "$x+$y = [expr $x+$y]"
}
```

该程序将输出：

```Tcl
1+2 = 3
3+4 = 7
5+6 = 11
```

 基本上当用户通过N个变量的列表时，对于每次迭代，N个新元素被消耗分配给指定的变量，脚本被执行。  **Foreach**可以做更多的事情，接受比以下示例中的变量/列表对更多： 

```Tcl
set firstlist {1 2 3 4 5 6}
set secondlist {a b c d e f g}
foreach {a b} $firstlist {c d} $secondlist {
	puts "$a$c,$b$d"
}
```

这次输出将是： 

```
1a,2b
3c,4d
5e,6f
```

这是简单的使用，但是多个列表并行走。 正如你可以看到，foreach是一个非常灵活的命令，用于Tcl中心的数据结构，因此您将在几乎每个非常简单的Tcl程序中看到此命令。 

## lrange命令

  **Lrange**从列表中提取一个子列表。 要提取的子列表由开始和结束索引指定，索引通常为零，并且可以使用“end- <integer>”符号。 例： 

```Tcl
% lrange {Tcl is a programmable programming language} 2 end-1
a programmable programming
%
```

## lappend命令

在这一点上，我们知道一些操作在列表上的命令： **length** ， **lindex** ， **foreach** ， **lrange** 。 他们有一些共同之处，因为所有的都将Tcl列表作为输入。  **lappend**命令替代地使用包含列表的变量的名称作为其第一个参数，以及可变数目的其他参数。  **Lappend**获取存储在指定变量中的列表，将列表中的每个参数附加（作为新列表元素的每个参数），并将该变量设置为新获取列表的值。 所获得的列表的值也作为**lappend**的返回值返回：

```Tcl
% set a {foo bar}
foo bar
% lappend a biz buz
foo bar biz buz
%
```

 如果指定的变量不存在，它将被创建（并且lappend将假定以前的内容是空列表）。 

 使用lappend，我们可以创建给定输入列表的用户定义的过程，返回由相同元素组成的列表，以相反的顺序。 这个程序的一个好名字是**lreverse** ，写得很简单： 

```Tcl
proc lreverse l {
    set result {}
    set i [llength $l]
    incr i -1
    while {$i >= 0} {
	  lappend result [lindex $l $i]
	  incr i -1
    }
    return $result
}
```

用法也很简单：

```Tcl
% lreverse {a b c}
c b a
```

 在**lreverse**实现中，有两个我们从未使用过核心命令。 第一个是自增**incr**，第二个是**while** 。  **Incr**非常简单，它需要两个参数：一个变量的名称和一个整数。 它获取必须是整数的变量的值，将第二个参数添加到其中，并将结果设置为变量的新值。 

 如果变量a设置为6，则命令“incr a 2”将其设置为8，而“incr a -1”将设置为5，依此类推。 

 第二个参数**虽然**有两个参数，第一个是一个表达式，第二个是脚本。 该表达式继续执行时执行该脚本。 表达式，就像对于**if**命令一样，在内部使用**expr**命令进行评估。 

## lset命令

  **lset**命令在某种程度上是对**lindex**的补充。 当**lindex**返回指定索引**处**的list元素时，lset将使用新值替换指定位置处的list元素。 像**lappend一样** ， **lset**将输入一个包含列表的变量名称，并直接修改变量的值，而不是返回修改的列表。 使用**lset的**一个例子： 

```Tcl
% set a {a b c d}
a b c d
% lset a 2 X  
a b X d
% puts $a
a b X d
```

  **Lset**可以以类似于**lindex的**方式直接访问嵌套列表，指定多个索引，例如命令“lset foobar 2 3 $ newval”将$ newvalue设置为列表中第三个元素，该列表是第变量$ foobar。 

## lsort命令

 如名称所示， **lsort**在输入中输入一个列表，并将其作为返回值返回。 命令结构是： 

```Tcl
lsort ?options? list
```

请注意，在Tcl中，广泛使用在两个问号包裹显示的命令结构，这些参数在实际的命令使用中可以被省略（实际上这意味着**lsort**命令能只使用一个参数：list ）。

选项只是对lsort命令具有特殊意义的字符串，并且以T字符命令为前缀“ - ”字符（但请注意，lsort知道除最后一个之外的所有选项都是参数）。 选项的目标是修改lsort排序的方式，并且可以组合以获得特定的效果。 不是所有的选项将在这里重新出现，请查看手册页，如果你想要的细节。

一些例子：

```Tcl

% set mylist [list this Tcl book have more than 2 chapters and 10 pages]
this Tcl book have more than 2 chapters and 10 pages
% lsort $mylist
10 2 Tcl and book chapters have more pages than this
% lsort -decreasing $mylist
this than pages more have chapters book and Tcl 2 10
% lsort -dictionary $mylist
2 10 and book chapters have more pages Tcl than this
% 
```

 第一个例子调用**lsort**没有选项，但只要列表排序：如果以这种方式调用**lsort**排序列表比较使用ASCII值的字母（实际上使用unicode，但现在你可以忽略这个特别是如果你不使用unicode字符串）。 

 第二个示例添加了*-decreasing*选项来反转排序顺序。 第三个例子改变了指定-dictionary选项的*比较算法* ：这种模式对于许多用途来说更为智能，因为它忽略了字母的情况，并且能够更好地处理数字（如果一个字符串包含一个数字，数字的值是比较价值）。 这个效果类似于字典中引语的顺序，因此是名字。 

  lsort的一个有趣的**lsort**是由于*-unique*选项，只要重复一次，返回每个元素一次： 

```
% lsort -unique {foo bar bar bar x y y y z foo}
bar foo x y z
```

 将这个功能直接包含在一个库中的Tcl过程叫做“ **unique**就可以很舒服。 实现简单：

 ```Tcl
proc unique l {
	lsort -unique $l
}
 ```

## 列出与变量名称相对应的值

 也许现在提问有关Tcl的复杂问题为时尚早，但有些读者可能会想知道为什么有一个Tcl列表命令可以正常工作，直接在列表的值上运行，以返回另一个列表作为结果。存储列表的变量的名称。 

 现在知道有效率的原因是足够的，因为一种方法比另一种方法更方便。 第二个原因是，实际上对于已经存储在变量中的列表使用**lset**和**lappend**更为常见。 

 当然，由于它是Tcl的标准，如果您需要使用与值相似的程序，则可以实现它们。 实际上已经有可以用于范围的程序，即可以**lreplace**和**concat**。 如果您有兴趣，请查看此功能的手册页。 作为示例，以下是与**lset**类似的函数，它将参数作为参数，列表，索引和新值，并返回一个列表，其中指定的索引将替换为新值： 

```Tcl
proc mylset {mylist index newval} {
    lset mylist $index $newval
}
```

 它是用lset编写的，因为lset已经返回修改的列表的值。 用法很简单： 

```Tcl
% mylset {a b c d} 2 foo
a b foo d
% 
```

 请注意， **mylset**不会以列表作为参数传递的任何方式进行更改。 在Tcl中，procedure的参数是局部变量，参数按值传递。 注意， **mylset**不会将存储的列表更改*为*变量： 

```Tcl
% set a {a b c d}
a b c d
% mylset $a 2 foo
a b foo d
% set a
a b c d
```

  *一个*变量继续包含列表**abcd** ，Tcl是一个安全的语言，不可能改变作为参数传递的变量的值， **除非**过程以特殊的方式写入，但在这种情况下，用户必须通过变量的名称，而不是使用变量名之前的**$**字符替换值。 我们会在以后更好地看到这些事情。 

# 字符串

 本章展示了有趣的Tcl命令，用于对字符串进行基本字符串操作，字符串匹配，正则表达式，字符串转换为列表，反之亦然。  Tcl上的字符串相关命令集很大，你可以猜到，这是对语言本身的语义特别重要的字符串，而不仅仅是数据类型。 幸运的是，这是更好的组织语言的一部分，所以很多命令不难记住。 

## append命令

  **append**命令非常类似于**lappend** ，而是将元素附加到列表中，它将字符串附加到字符串。 命令的结构是： 

```Tcl
append varName ?value value ...?
```

  *varName*之后的每个参数都追加到*varName*变量的当前内容中，并返回该变量的新内容。 例： 

```Tcl
% set s "foo"
% foo
% append s bar
% foobar
% append s x y z [string length $s]
% foobarxyz6
```

  **append**命令非常有效，写入“附加$ b”然后“设置$ a $ b”更快，但这两种解决方案都可以正常工作。 在使用非常高级编程语言（如Tcl）进行编程时，考虑速度问题仍然有点习惯，因为它们不像C等低级语言那么快。 

## string命令

 而不是使用不同的命令来执行不同的字符串操作Tcl使用一个名为**string**的单个字符串操作命令，它将操作作为第一个参数。 其余参数与执行操作有不同的含义。 在Tcl俚语中，不同的操作称为*子命令* 。 

 例如要获取字符串的长度，提供给**字符串**命令的第一个参数是*length* ，即要执行的操作的名称，如果您愿意， *则为子命令* 。 另一个参数是字符串本身。 

```
% string length "Tcl is a string processor"
25
%
```

 数字25当然是字符串“Tcl是字符串处理器”内的字符数。 重要的是要知道Tcl字符串是*二进制安全的* ，所以每种字符都可以在一个字符串中，包括值为零的字节： 

```
% string length "ab\000xy"
5
```

最好理解这个概念，因为在Tcl编程中，不只有当您需要读取文本文件时，才会使用字符串，当涉及二进制数据时也会使用字符串。 

**string**命令有许多其他**子**命令，我们将显示一个子集，包括本章中更有趣的内容。 

## string range

  **range**子命令用于提取字符串的部分。 它的工作方式与**lrange**命令非常相似。 索引也可以是end- <index>的形式。 正式的命令结构是：

```Tcl
string range string start-index end-index
```

 例：

```Tcl
% string range "Dante Alighieri is a Tcl user" 7 end-10
Alighieri is
%
```

## string index 

**index**子命令只从整个字符串中提取单个字符。

```Tcl
string index string index
```

 例： 

```Tcl
% string index "foobar" 3
b
% string index "foobar" end
r
```

 作为**string index**命令的更有趣的真实应用程序是以下过程，它反转字符串中字符的顺序，转换为“lcT”中的“Tcl”。 因为最后一个字符串被*反转，*所以这个过程被称为**stringReverse** 。 

```Tcl
proc stringReverse s {
    set res {}
    for {set i 0} {$i < [string length $s]} {incr i} {
        append res [string index $s end-$i]
    }
    return $res
}
```

 即使您将程序输入到文件中，例如rev.tcl，仍然可以使用*tclsh*进行一些使用**source**命令进行交互式实验的测试。 

```Tcl
% source stringReverse.tcl 
% stringReverse "string to reverse"
esrever ot gnirts
% 
```

**source**命令告诉Tcl执行指定文件的内容，代替它被输入。 所以在“source stringReverse.tcl”调用之后，过程stringReverse被定义并且可以被调用。

```Tcl
% source stringReverse.tcl 
% stringReverse "string to reverse"
esrever ot gnirts
% 
```

## string equal

 发生频繁的操作是比较两个字符串。  **String equal** ，它搜索**完全**匹配，也就是说，字符串必须匹配逐字符，以被认为是相同的命令。 如果作为值传递的两个字符串相同，则返回值为1，否则返回0： 

```Tcl
% string equal foo bar
0
% string equal tcl tcl
1
% string equal tcl TCL
0
```

  “tcl”和“TCL”对于字符串相等是不一样的。 如果要以不区分大小写的方式进行比较，则会有一个*-nocase*选项来更改行为并考虑不同情况下的字符相同： 

```Tcl
% string equal -nocase tcl TCL
1
```

 另一个有趣的选项是*-length num* ，它限制与第一个*num*字符的比较： 

```Tcl
% string equal Petroleum Peter
0
% string equal -length 3 Petroleum Peter
1
```

 两个选项*-nocase*和*-length*可以组合。 

## string compare

 这个子命令非常类似于**equal** ，但是如果字符串相同，则返回true或false，则该命令将返回： 

```Tcl
-1 if the first string is < than the second
0  if the first string is the same as the second
1  if the first string is > than the second
```

与**string equal** ，这可能会对排序或其他任务有用。

## string match

 当需要更强大的字符串匹配能力时，可以使用**字符串匹配**代替**字符串相等** ，因为为了比较两个字符串，该命令会将字符串与*模式*进行比较。 

 字符串匹配支持由正常字符组成的模式，以及以下特殊序列： 

  *匹配任何字符序列。 即使是空字符串。  ？ 匹配任何单个字符。  [chars]匹配指定的角色集合。 可以在xy形式中指定一个点，如[az]，它将匹配从*a*到*z的*每个字符。  \ x完全匹配*x，*而不用特殊的方式解释它。 这是为了匹配*，？，[，]，\，作为单个字符。 

 这是模式的一个例子，它可以匹配什么，以便使它更容易理解它的工作原理： 

> ```
> *xyz*         can match xyz, fooxyz, fooxyzbar, and so on.
> x?z           can match xaz, xxz, x8z, but can't match xz.
> [ab]c         can match ac, bc.
> [a-z]*[0-9]   can match alf4, biz5, but can't match 123, 2foo1
> ```

**字符串匹配**的命令结构是： 

```Tcl
string match ?-nocase? pattern string
```

 如果*字符串*匹配*模式* ，返回值分别为1或0。  -nocase选项可以用于在匹配时不关心的情况。 例： 

```Tcl

% string match {[0-9]} 5
1
% string match foo* foo
1
% string match foo* foobar
1
% string match foo* barfoo
0
% string match ?*@*.* antirez@invece.org
1
% string match ?*@?*.?* antirez@invece.org
1
% string match ?*@?*.?* antirez@org
0
% 
```

 请注意，包含[xy]形式的模式必须使用大括号分组，或使用\引用，以防止Tcl尝试将其替换为命令。 

 该示例中的最后一个模式显示了如何匹配任何东西至少N个字符的长度使用N个问号后跟一个星号。  “*”将至少匹配3个字符，依此类推。  Tcl支持使用正则表达式更高级的模式匹配，仍然**字符串匹配**是非常有趣的，因为在大多数情况下，足以以更简单的方式表达模式，并且比正则表达式命令工作得更快。 

##  string map

**String map**是一个强大的工具，可以用其他字符串替换字符串的出现。 替换由键值对列表驱动。 例如，列表{foo bar x {} y yy}将用“bar”替换每次出现的“foo”，将删除每次出现的“x”，并将每次出现的“y”复制。 命令结构如下：

```Tcl
string match ?-nocase? pattern string
```

 替换以有序的方式完成：从原始字符串的第一个字符开始，搜索键值对列表中的每个键。 如果没有匹配，则该字符将附加到将被返回的结果，并且该过程从下一个字符继续。 如果有匹配，则相对于匹配键的值将追加到结果中，并且该过程从匹配键之后的字符继续。 

 上述描述可能会出现迂回和复杂，实际上了解**string map**如何工作并不困难。 它将键值对中的每个键的发生转换为相应值的出现。 一旦程序员可以使用**string map** ，他可能会想知道替换过程的细节，所以上面的文本将在以后更加有用，当你将是一个更有经验的Tcl程序员。 

 例子： 

```Tcl
% string map {x {}} exchange
echange
% string map {1 Tcl 2 great} "1 is 2"
Tcl is great
```

 请注意， **string map**在原始字符串上仅迭代一次，因此模式不能作为早期替换的效果匹配： 

```Tcl
% string map {{ } xx x yyy} "Hello World"
HelloxxWorld
```

 当键值对列表不是常数时，最好使用**list**命令创建它： 

```Tcl
% set a foo
foo
% set b bar
bar
% string map [list $a $b $b $a] foobar
barfoo
% 
```

## string is

 **String is**测试字符串是否是给定类的成员，如整数，字母数字字符，空格等。 命令的结构是： 

```Tcl
string is class ?-strict? ?-failindex varname? string
```

 默认情况下，该命令为空字符串返回1，因此*-strict*选项用于反转行为并返回空字符串0（即不考虑空字符串给定类的成员）。 

  *该类*可以是以下之一： 

> ```
> alnum                 alphabet or digit character
> alpha                 alphabet character
> ascii                 every character in the 7-bit ASCII range
> boolean               any form allowed for Tcl booleans (0, 1, yes, no, ...)
> control               a control character
> digit                 a digit character
> double                a valid Tcl double precision number
> false                 any form allowed for Tcl boolean with false value
> graph                 a printing character, except space
> integer               any valid form of 32-bit integers
> lower                 a lovercase alphabet character
> print                 a printing character including space
> punct                 punctuation character
> space                 any space character
> true                  any form allowed for Tcl boolean with true value
> upper                 an uppercase lphabet character
> wordchar              any word character. alphanumeric, puntuation, underscore
> xdigit                an hexadecimal digit
> ```

 正如你可以看到一些类面向单个字符（如alnum），有些类对字符串（如整数）有用。 如果由多个单个字符组成的字符串针对面向字符的类进行测试，则字符串的每个元素都必须属于要返回的命令的类1.一些示例： 

```Tcl
% string is integer 33902123
1
% string is integer foobar
0
% string is upper K
1
% string is lower K
0
% string is upper "KKK"
0
% string is upper "KKz"
0
```

 如果使用*-failidnex*选项后跟变量的名称，则该命令将将测试失败的第一个字符的索引存储在变量中。 

## 更多字符串子命令

 我们没有覆盖大量的**string**命令。 读者可能希望查看**string **手册页以查看可用内容：了解使用内置Tcl功能可以做些什么来避免重新实现已有的功能非常重要。 

## 高级字符串匹配

  Tcl字符串匹配功能包括两个强大的命令[regexp]和[regsub]，以利用类似于egrep的**正则表达式**功能。 本命令将在本书“FIXME”一章中进行探讨。 

# 列表和行

 因为在Tcl中，列表是一个这样的中央数据结构，所以在列表操作方面编写程序往往很有意思，即使目标是使用不同类型的数据。 在字符串上执行复杂操作时，这是非常真实的：而是直接处理字符串，可以将字符串转换为列表，对结果列表执行操作，然后将其转换为字符串。 将列表中的字符串转换为自然的方法是将其转换为列表，其中原始字符串的每个字符都是列表的元素。 在Lisp方言中常常使用相同的概念，如Scheme编程语言。 在继续探索这个强大的编程概念之前，您需要学习两个新的Tcl命令，以便能够将一个字符串转换成一个列表，反之亦然。 这两个命令是**split**和**join** 。

## 将字符串转换为列表 

 **split**命令将一个字符串转换为一个列表，其中列表中的每个元素都将被分割成字符串。 分割字符串的位置是根据一组字符指定的：该字符串在每个出现指定字符的位置分割。  **split**命令的结构是： 

```Tcl
split string ?splitChars?
```

 如果省略*splitChars* ，则它默认为单个空格字符，因此字符串将在列表中转换，其中每个元素都以原始字符串中的空格分隔。 

 例如，“abracadabra”可以分割成六个元素列表，使用“a”作为*splitChars*参数： 

```Tcl
% split abracadabra a
{} br c d br {}
```

 注意列表的第一个和最后一个元素是空字符串，因为我们拆分的字符串以*一个*开头和结尾。 拆分代替*b* ，结果如下：

```Tcl
% split abracadabra b
a racada ra
```

 我们获得了三个元素列表。 如果我们将*b*和*c*指定为分割字符呢？ 

```Tcl
% split abracadabra bc
a ra ada ra
```

字符串被分割在有*b* **或** *c*字符的地方，而不是字符串“bc”出现在字符串中。 下图显示了上一个**split**示例中的拆分点：

```Tcl
abracadabra
 |  |   |
 0  1   2
```

  *splitChar*在示例中为“bc”，因此字符串在有*b*或*c*字符的位置被拆分。 因为字符串中有4个字符是*b*或*c* ，所以有4个分割点，如图所示，所以列表的元素是“a”，“ra”，“ada”，“ra”。 

 作为拆分的使用示例，我们可以尝试解析Unix系统中存在的/ etc / passwd文件的一行。 系统中的第一行如下所示： 

```
root:x:0:0:root:/root:/bin/zsh
```

假设我们需要得到第五个字段，该怎么做呢？ 最简单的解决方案之一是将字符串转换为元素列表，使用“：”作为元素分隔符，然后使用lindex获取第五个元素。

```Tcl
root:x:0:0:root:/root:/bin/zsh
% lindex [split $line :] 5
/root
```

 **split**输出产生用作**lindex**输入的列表。 这个代码在Tcl中是惯用的，是解析包含由单个字符分隔的文件的许多字符串的最简单的方法。 

 请注意，有一类字符串，元素之间的分隔符只是一个空格，如“这是一个字符串”。 您可能会想知道为什么在这种情况下使用**拆分**是有用的，因为这个字符串实际上是一个有效的Tcl列表，因此可以直接使用**lindex**或其他列表操作命令： 

```Tcl
% lindex "this is a string" 1                        
is
```

 但是这是一个简单的例子，因为有一些字符串包含空格分隔的字段， **这些**字段**是**无效的Tcl列表： 

```Tcl
% set mystring "this is { a string"
this is { a string
% lindex $mystring 1
unmatched open brace in list
while evaluating {lindex $mystring 1}
```

 因为$ mystring没有包含一个有效的Tcl列表（有一个开放的分支，而不是相应的关闭的），调用**lindex**对它的结果是一个错误。 另外请注意，即使关闭括号仍然意义可以不同：如果字符串是一个空格分隔的字段列表， *{*字符应该只是一个像任何其他元素。 

 要解决所有这些问题，只需像以前一样对/ etc / passwd行调用与空格分隔的字符串进行**split** ： 

```Tcl
% set mystring "this is { a string"
this is { a string
% lindex [split $mystring] 2
{
```

## 从字符串到字符列表

 当使用空字符串调用**split**命令作为*splitChars*参数时，该命令的行为是转换列表中的字符串，其中原始字符串的每个字符现在都是列表的元素： 

```Tcl

% split "foobar" {}
f o o b a r
```

 这允许程序员操作字符串作为我们将在本章后面看到的列表，但是在介绍这个重要的参数之前，最好理解反向转换是如何完成的：如何将列表转换成字符串。 

## 将列表转换为字符串

  **join**命令是**split**的补充。 它将列表的每个元素连接到一个字符串中，使用另一个字符串作为元素之间的分隔符。 命令结构是：

```Tcl

join list ?joinString?
```

   *joinString*参数可以省略，默认为null。 命令使用非常简单： 

```Tcl
% join [list 1 2 3] .
1.2.3
% join [list 1 2 3] "000"
100020003
% join [list 1 2 3] {}
123
```

```

```

 最后一个例子很有趣，因为与使用空分隔符分割字符串相反。 如果列表的每个元素都是使用与空字符串**连接**的字符，则*joinString*参数将将列表转换为字符串。 

 连接的另一个有趣的用法与**expr**有关：通常，您可能有一个包含数字的Tcl列表，您可能想要计算列表中所有数字的总和。 你可能认为的第一个解决方案是这样的： 

```Tcl
set list {1 2 3 4 5}
set sum 0
for {set i 0} {$i < [llength $list]} {incr i} {
	incr sum [lindex $list $i]
}
```

这或多或少的类似你应该如何在C中使用的方法，但在Tcl还有其他更舒适的解决方案。 一个涉及使用**join** ，使用“+”字符串作为元素分隔符： 

```Tcl

% join {1 2 3 4 5} +
1+2+3+4+5
```

 您可以看到，生成的字符串是一个有效的**expr**表达式 ，因此我们可以以更为巧妙的方式重写上述代码： 

```Tcl
set list {1 2 3 4 5}
set sum [expr [join $list +]]
```

## 将字符串当作列表操作

 现在我们有足够的知识将一个字符串转换成一个列表，然后再回到一个字符串。 这个简单的事实是非常强大的，因为Tcl的列表相关命令在某些方面比字符串命令**要强大得多** ：例如， **foreach**或**lsort**命令对于字符串没有等效。 

 让我们从一个真实的例子开始：目标是编写一个给定一个字符串的Tcl脚本，返回一个由字符串组成的字符串，该字符串只显示在字符串上，只有一次，这是为了写这个单词所需的字母。 例如，对于字符串`apple`，字母表是`aelp`，而对于字符串`Tcl is cool`，字母表是`cilost`。 这是我们需要写的简单代码来执行操作： 

```Tcl

% join [lsort -unique [split "supercalifragilistichespiralitoso" {}]] {}
acefghiloprstu
```

 所以单词`supercalifragilistichespiralitoso`（意大利语中的世界），只有14个不同的字符令将字符串转换为由-lsort使用**-unique**选项处理的字符列表：此命令的效果是删除重复的元素并对列表进行排序。 然后使用**join**命令将此新列表转换回字符串。 

我们可以使用一种简单的技术来编写一个程序来测试两个字符串是否是另一个字符串： 

```Tcl
proc isanagram {word1 word2} {
    set sorted1 [join [lsort [split $word1 {}]] {}]
    set sorted2 [join [lsort [split $word2 {}]] {}]
    string equal $sorted1 $sorted2
}
```

 这一次我们不使用lsort的-unique选项，所以*ordered1*和*ordered2*将只包含一个字符串，其中*word1*和*word2*的字符被排序：如果两个字符串由两个字符串的排序版本完全相同的字母组成将是一样的 

 例如，`more`的卦是`rome`，我们可以直接使用tclsh来检查它是否正确： 

```Tcl
% join [lsort [split rome {}]] {}
emor
% join [lsort [split more {}]] {}
emor
```

 两个字的排序版本是一样的。 如果*word1*是*word2*的卦**义，**则程序**isanagram**返回1，否则返回0，因为它的最后一行，其中**string equal**两个排序的字比较（记住，如果有nnn，Tcl过程返回执行默认的最后一个命令的返回值执行路径中的**return**命令）。 

 与**lsort**和**split** 相似 ， **foreach**命令可以用于字符列表。 为了迭代一个字符串的每个字符，使用这种简单的例子： 

```Tcl
% foreach x [split "mystring" {}] {puts $x}
m
y
s
t
r
i
n
g
```

 但是，如果需要，没有理由不利用foreach的全部力量。 以下程序反转字符串中每两个字符的位置： 

```Tcl
% set var {}
% foreach {a b} [split "mystring" {}] {append var $b$a}
% set var  
ymtsirgn
% 
```

  Tcl字符串是二进制安全的，所以例如你可以使用上面的代码翻译一个由16位数字组成的文件，这个16位数字存储在一个小字节，以16位数字存储在大字节。 

 另一个例子涉及使用我们之前写过的一些部分的**lreverse**命令（它只是返回一个输入列表的一个反转版本，其顺序为元素的顺序）： 

```Tcl
% set string "hello world"
hello world
% join [lreverse [split $string {}]] {}
dlrow olleh
```

 这是一个舒适的方式来反转字符串。 

 最后一个例子有点复杂，但也可能更有趣。 假设你有两个字符串，并想知道两个字符串是否至少有一个共同的字符。 使用Tcl的列表命令和将字符串转换为列表的能力，可以以紧凑的形式编写这样的代码： 

```Tcl
proc commonChars {a b} {
    set a [split $a {}]
    set b [split $b {}]
    set union [concat [lsort -unique $a] [lsort -unique $b]]
    expr {[llength $union] != [llength [lsort -unique $union]]}
}
```

  **concat**命令使用空格作为分隔符将字符串连接在一起：如果字符串是有效列表，则生成的字符串将是一个有效的列表，因此可以使用**concat**创建一个列表，该列表是更多列表的并置。 现在你知道这个新的命令，不应该很难理解**commonChars**命令的工作原理：前两行将'a'和'b'转换成字符列表。 第二行将列出在变量“union”中连接“a”和“b”的列表。 这只是一个字符列表，其中包含“a”和“b”的两个字符。 如果'a'和'b'具有共同的字符，则命令[lsort-unique $ union]将使列表更短（因为有重复的元素），所以过程的最后一行将原始列表的长度“union”，删除重复元素后的列表长度。 如果两个长度相同，则两个字符串不具有共同的字符，否则它们具有。 

 这就是用法： 

```Tcl
% commonChars foo bar
0
% commonChars tcl char  
1
```

 就这样。 我希望本章显示字符串和列表在Tcl中是非常相关的，这可能是一个好主意，转换字符列表中的字符串，以利用与Tcl相关的强大的列表相关的命令。

# 更多程序

 Tcl程序对于不是新手读者来说，我们已经有机会使用**proc**命令编写一些简单的**过程** 。 仍然有必要调查的细节，以便能够编写非常简单的程序。 本章将介绍什么是Tcl过程的*局部变量* ，如何使用可变数量的参数编写过程，使用默认参数，如何编写递归过程，以及最后是什么以及如何从Tcl过程访问*全局*变量。 

## 局部变量

  Tcl程序可以使用**set** ， **append** ， **lappend**等命令创建新的变量。 如果在一个过程中创建一个变量，那么这个变量就被称为*局部变量* ，他的可见度和生命与过程调用密切相关。 

 以下**foobar**过程创建两个名为*a*和*b的*变量，并返回一个二元素列表，其值为*a*作为第一个元素， *b*值作为第二个元素： 

```Tcl
proc foobar {} {
    set a foo
    set b bar
    list $a $b
}
```

 每次调用该过程（没有任何参数）时，它将返回两个元素列表“foo bar”。 为了理解一个局部变量是什么，我们将尝试*在*每次调用**foobar**时执行*Tcl中*发生的事情：第一个事件是执行命令**set foo** ，该命令创建一个局部变量字符串“foo”作为内容。 当执行**foobar**过程的第二行，创建局部变量*b*时也会发生相同的情况。 最后，两个变量的值用作**list**命令的参数。 因为**list $ a $ b**是**foobar**过程的最后一个命令，所以Tcl解释器现在可以将其值返回给调用者：该过程可以退出。 局部变量*a*和*b*会发生什么？ 它们只是被破坏，值被丢失，并且每次调用**foobar**过程时**都会**发生这种情况，对于每个调用，这两个变量被创建然后被销毁。 

 所以我们可以说：在Tcl中，局部变量是在一个过程中创建的变量（即一个过程运行时），一旦创建过程准备好返回到调用者，就会被破坏。 

 到目前为止，我们知道一个局部变量的生命周期，但是它的访问能力呢？ 一个局部变量只能通过创建它的过程中的代码进行访问， *换句话说* ，局部变量的*可见*性仅限于创建过程（实际上这个规则有一个重要的例外，但是您需要等待下一章才能知道更多）。 

## 顶级

 前面的部分解释了在过程中创建的变量的行为，但实际上在Tcl中，可以在一个称为**顶级**的上下文中的程序外部运行代码。 例如，当您启动*tclsh*并写入一些Tcl命令时，该命令在顶层执行。 一般来说，没有出现在程序中的每个Tcl代码都处于顶级。 以下面的程序为例： 

```
puts "Here the code is running at top level"
proc foo {} {
	puts "But not here!"
}
foo
```

程序在顶层调用**puts**命令，然后调用**proc**命令，仍然在顶层，最后调用**foo**命令。 在这一点上，我们在**foo**函数内（不再在顶层）。 因为toplevel是一个Tcl自动调用来启动程序的过程，并且在程序运行时永远不会返回，在顶层创建的变量永远不会被销毁。 这个变量称为*全局变量* 。 

## 全局变量

 实际上，全局变量有些特别，不但不会被破坏（除非程序通过**unset**命令显式地破坏一个全局变量），还可以从Tcl程序创建和访问（即可以创建或访问全局变量，不在顶级）。 为了使其成为可能，使用**全局**命令。 在第一个例子中，我们将使用**set**在顶层创建一个全局变量，然后我们将使用**全局**命令从一个过程中访问它： 

```Tcl
set PI 3.1415926536

proc area radius {
    global PI
    expr $radius*$radius*$PI
}
```

 顶级**设置**命令创建包含*PI的*近似值的全局变量PI。  **区域**过程使用全局变量来计算具有给定半径的圆的面积（作为**区域**过程的唯一参数传递）。 在命令**全局PI**被调用后， **区域**过程可以自由使用*PI*变量。 

 过程也可以以类似的方式创建一个全局变量： 

```
proc createPI {} {
    global PI
    set PI 3.1415926536
}

createPI
puts $PI
```

 你可以猜到，这个程序的输出是“3.1415926536”。  **createPI**命令创建PI全局变量，可以通过**puts $ PI**命令从顶层直接访问PI全局变量。 

 重要的是要明白，虽然全局变量是有用的，以便采取程序的一些重要的状态，明智的用法是raccomended：使用全局变量的程序往往在其他情况下不太可重复使用，具有微妙*的副作用* ，一般来说在编写干净可读的代码的过程中，不但有很大的帮助。 

## 程序参数和按值传递

 程序的参数是一种特殊的局部变量。 这个变量唯一特别的是每次调用过程时都会自动创建它们的值，并将其值设置为传递给该命令的相应参数的值。 看下面的代码： 

```Tcl
proc myproc x {
    set x ""
}
set list "1 2 3 4 5"
myproc $list
puts $list
```

 程序**myproc**采用一个参数*x* ，并将其设置为空字符串。 这只是一个虚拟函数，仅适用于此示例。 代码创建一个包含五个数字列表的变量*列表* ，然后调用**myproc $ list** 。 在这一点上会发生什么？  **$ list**被扩展为它的值，然后调用**myproc**过程，所以就像直接调用： 

```Tcl
myproc "1 2 3 4 5"
```

 这意味着我们总是传递字符串作为过程参数。 在这一点上， **myproc的**执行开始：它首先做的是创建一个局部变量*x* ，这是函数的参数，将“1 2 3 4 5”设置为该变量的值。 然后**myproc**函数的第一行将空字符串设置为*x*变量的值。 最后， **$ list**命令在屏幕上打印*列表*变量的值，因此程序将输出“1 2 3 4 5”。 

 这里有什么意义 在Tcl中，程序的参数总是*通过值传递* 。 这意味着**myproc**不能更改*列表*变量的值：该值刚刚展开，传递给myproc，然后设置为*x*参数。 除非一个过程需要**一个变量的名称**作为参数，而不是一个值，否则总是安全的，该过程不应该改变调用者变量的值。 

 这使得Tcl编程非常安全，您可能不会在**内部重新**计算mystrangeprocedure，您**知道**以下代码片段不能更改存储在*l*变量中的列表的值： 

```Tcl
set l [list foo bar]
mystrangeprocedure $l
```

  **mystrangeprocedure**可以混淆其参数尽可能多的喜欢，但仍然变量*l*的值将继续是一个两个元素列表“foo bar”。 

 在这一点上，您应该想知道如何在Tcl中编写一个类似于**incr**命令的过程，能够增加一个居住在调用者程序上下文中的变量。 在下一章中，我们将看到，由于这种语言提供的极大的灵活性和内省性，Tcl规则如何被简单粗暴。 

## 具有可变数量参数的过程

 记住我们尝试写一个**+**程序？ 这是我们在上一章中写的代码： 

```Tcl
% proc + {a b} {expr $a+$b}
% + 3 4
7
```

 这个代码是可以的，但只用两个参数。 如果我要求三个数字，而不是写**+ 1 2 3**我必须写**+ 1 [+ 2 3]** 。 我相信你不喜欢这个限制，我同意。 在Tcl中编写具有可变数量参数的过程非常简单，所以我们可以编写一个新版本的**+**程序，可以从1到无限参数接受。 这是代码： 

```Tcl
proc + {x args} {
    foreach e $args {
        set x [expr $x+$e]
    }
    return $x
}
```

我们可以通过使用*tclsh*进行交互式测试，以确保它按预期工作：

```Tcl
% + 10 20
30
% + 1 2 3 4 
10
% + 50
50
% 
```

 现在看看它是如何工作的 正如你可以看到**proc**命令被调用的{x args}作为第二个参数（这是我们正在创建的过程的参数列表）。 第二个参数是*args* ，它是一个特殊的参数：如果参数列表的最后一个参数完全是字符串*args* ，则该函数可以接受无数个参数，这些参数是在过程中作为列表存储到*args*参数中叫做。 

 在**+**过程的例子中，如果我们用一个参数调用它，它将被分配给*x* ，并且*args*将是一个空列表。 如果我们用两个参数来调用它，那么第一个被赋值给*x* ，而第二个被赋值给列表的唯一元素*args* ，依此类推。 以下是传递给**+**的不同数量*参数的* *x*和*arg*将包含的表： 

```Tcl
+                  ;# error, wrong number of arguments for procedure
+ 10               ;# x <- "10", args <- ""
+ 10 11            ;# x <- "10", args <- [list 10 11]
+ 10 11 20         ;# x <- "10", args <- [list 10 11 20]
```

 这就是为什么**+**过程的实现使用**foreach**来遍历参数列表，将所有参数添加到第一个参数（包含在*x中* ）。 

 请注意，只要*args*显示为最后一个参数，可以提供任何数量的正常参数（包括零），例如{xyz args}是一个有效的参数列表，用于需要3到无数个参数的过程。 前三个参数将被分配给*x* ， *y* ， *z* ，所有剩余的参数将被放在列表中并分配给*args* 。 

## 具有默认参数的过程

  **proc**命令的另一个有用的工具是使用*默认参数*编写**过程**的能力。 调用该过程时可以省略默认参数，默认值在缺省值为创建过程时指定的值。 有很多情况下，这是可取的，我们已经显示了使用此功能的核心命令（例如在用于将列表的元素连接到字符串中的**join**命令中， *joinString*参数可以省略，默认为单个空间）。 

 作为一个例子，我们可以编写一个将列表的每个元素增加一个的过程： 

```Tcl
proc lincr l {
	set result {}
	foreach e $l {
	    lappend result [expr $e+1]
	}
	return $result
}
```

 这是一些输出： 

```Tcl
% lincr {10 20 30}
11 21 31
% lincr {5 6}
6 7
```

 请注意，我们需要将*结果*变量初始化为空列表，以确保函数在输入为空列表时工作良好，否则*结果*变量可能无法创建，因为**lappend**将永远不会被调用， **return $ result**将生成一个错误。 

 如果我想增加元素10而不只是1，怎么办？ 我们可以重写这个函数，这样它会增加一个称为increment的参数，并在**foreach**循环中使用它的值创建新的字符串。 

```
proc lincr {l increment} {
	set result {}
	foreach e $l {
	    lappend result [expr $e+$increment]
	}
	return $result
}
```

 再次，这样做很好，将代码剪切并粘贴到*tclsh中*并尝试一下： 

```Tcl
% lincr {10 20 30} 1
11 21 31
% lincr {10 20 30} 5
15 25 35
```

 但是，不可思议的是，在一段时间后，您在程序中使用此过程，您发现80％的时间需要增加一个，为什么不能只写**lincr $ mylist**而不是**lincr $ mylist 1** ，并指定增量*只有当它*不同于普通的情况？ 这是默认参数在场景中输入的地方： 

```
proc lincr {l {increment 1}} {
	set result {}
	foreach e $l {
	    lappend result [expr $e+$increment]
	}
	return $result
}
```

 新版本与前一版本完全相同，但是现在是{l {increment 1}}的过程的参数列表有所不同。 简而言之，如果参数列表中的一个参数本身是两个元素列表，则第一个参数将被解释为参数的名称，第二个作为默认值，如果未指定，则赋予该参数。 现在函数可以接受一个或两个参数，只有一个参数会将列表元素增加一个，更多的参数将使用指定的值增加： 

```Tcl
% lincr {1 2 3}
2 3 4
% lincr {1 2 3} 10
11 12 13
```

 一个过程可能有多个默认参数，但它们都必须位于参数列表的末尾，您不能像{a {b 10} c}在中间添加默认参数，但可以使用多个默认参数例如{a {b 10} {c 20}}：在该示例中，如果您只指定一个参数，则b将默认为10，c至20，如果您指定一个参数将用于设置值的*b* ，最后如果你添加另一个它将被用于*c* 。 

 此规则的一个例外是，可以使用*args*特殊参数来编写具有可变数量的参数的过程，其中一些最后一个参数具有默认值： 

```Tcl
% proc foo {a {b 10} {c 20} args} {puts "$a - $b - $c - $args"}
% foo 5 
5 - 10 - 20 - 
% foo 5 1 2
5 - 1 - 2 - 
% foo 5 1 2 a b c d
5 - 1 - 2 - a b c d
%
```

 只是最后一个注释，如果默认值在变量内，或者想要在运行时用命令替换来计算呢？ 如果参数列表使用{}分组，则不会发生变量和命令替换，因此您需要使用list命令以不同的方式编写该过程，如下所示： 

```Tcl
set value 100
proc myproc [list a b [list c $value]] {
	puts "$a $b $c"
}
```

**proc**的第二个参数是一个列表，因此您可以在运行时创建它。 具有可变数量参数的默认参数和过程是非常重要的，因为编写需要较少键入基本情况的过程很好，并且在有意义的情况下可以接受无数个参数（如在**+**程序的情况下）。 

## 递归

  Tcl过程可以自己调用，这使得写入递归过程成为可能。 递归是如此重要，因为许多问题在一个简单的例子中表达是微不足道的。 递归过程的第一个例子用于计算整数列表的最大元素。 我们知道如何解决一个长度为1的列表（最大只是唯一的元素）的情况，并使用递归，我们可以解决列表中任意长度的问题： 

```
proc lmax l {
    if {[llength $l] == 1} {
        lindex $l 0
    } else {
        if {[lindex $l 0] > [lmax [lrange $l 1 end]]} {
	      lindex $l 0
	  } else {
	      lmax [lrange $l 1 end]
	  }
    }
}
```

 可以*读取*该过程：如果列表长度为1，则max是唯一包含的元素，否则将列表分为两部分，第一个元素和列表的其余部分。 如果第一个元素大于其余元素的最大值，则它是列表的最大值，否则列表的最大值是列表其余部分的最大值。 该过程将适用于每个非空的整数列表： 

```Tcl
% lmax {1 50 34 25 61 7 8 9}
61
% lmax {1 2 3}
3
```

 递归过程的另一个例子是经典*斐波纳契*函数，定义如下： 

```
FIB(1) = FIB(2) = 1
FIB(N) = FIB(N-2)+FIB(N-1) (for N > 2)
```

 斐波纳契函数的Tcl实现如下： 

```Tcl、
proc fib n {
    if {$n < 3} {
        return 1
    } else {
        expr {[fib [expr {$n-2}]]+[fib [expr {$n-1}]]}
    }
}
```

 在看程序细节之前，请注意，我们把**expr**的表达式放在大括号中。 你可能会想知道如果组合会阻止它，命令替换将会生效：诀窍是， **expr**命令执行自己的变量转换和命令替换到我们传递的参数，如果我们提供一个表达式，它会快得多与大括号分组。 所以要写`expr $a+$b`，你也可以编写`**expr {$a+$b}**` ，让编译器优化代码运行得更快。 

 关于**fib**程序，都应该是清楚的，它是*斐波纳契*函数的数学定义的一个简单的Tcl翻译。 有趣的是，这个功能将自动结束计算多次。 例如为了计算FIB（5），FIB（2）的值被计算3次，这不是最优的，因为FIB（2）将始终具有相同的值，所以有用的是计算一次。 我们将在接下来的章节中看到如何编写一个可以自动缓存递归过程的计算值的Tcl过程。 缓存已经计算的过程值的技术称为*记忆* ，由于Tcl的内省功能，我们将能够编写一个记忆过程，用作过程的第一个命令将把该过程转换成一个*记忆的过程* 。 

## 递归限制

 为了在太晚之前陷入无限递归错误，如果达到给定的递归深度（嵌套调用数），Tcl解释器将生成错误。 例如，以下过程将在调用时退出并显示错误。 

```Tcl
proc infinite {} {
    infinite
}
infinite
```

  **infinite**将永远称为**infinite** 。 

 这可能有时会造成问题，您可能需要编写执行递归的代码，深度不允许以默认限制。 为了更改此使用以下Tcl命令： 

```
interp recursionlimit {} $newlimit
```

 其中`*$newlimit$`是新递归限制的值。 如果你想检查当前的递归限制是什么，调用这个命令没有最后一个参数： 

```
% interp recursionlimit {}
1000
```

 请注意，放大此限制太多可能并不总是导致使用递归递归深度编写递归过程的能力：Tcl在C中实现，解释器调用自身，因此C堆栈本身可能会溢出。 如果发生这种情况，您将看到类似于*堆栈溢出*或*分段故障的操作系统错误* 。 

 有时可以在程序返回之前编写递归出现的过程，这称为*尾递归* 。 正如我们将在本书的高级章节中看到的，可以编写一个执行所谓的*尾递归优化*的**proc**版本，这样可以编写在恒定空间中运行的递归过程。 

# 控制结构

 在最后一章中，我们将重点放在了学习Tcl如何工作的目标，一些重要的核心命令避免了我们的注意。 现在读者应该熟悉Tcl程序的结构，无需太多的努力来学习一些新的有趣的命令。 本章涵盖的所有命令都是关于控制程序的流程。 我们已经看到， **if** ，**while** 和**foreach** ，但还有其他有趣的控制结构发现。 

## switch命令

 当我们要针对多个模式测试字符串的值，并根据匹配模式执行不同的代码时， **switch**命令非常有用。 您可以看到**switch** 作为一个特殊情况， **if**，例如代码：

```Tcl
proc number2word n {
    if {$n == 0} {
        return "zero"
    } elseif {$n == 1} {
        return "one"
    } elseif {$n == 2} {
        return "two"
    } else {
        return "hello!"
    }
}
```

  可以翻译成这个使用**switch**： 

```
proc number2word n {
    switch $n {
        0 {return "zero"}
	  1 {return "one"}
	  2 {return "two"}
	  default {return "hello!"}
    }
}
```

 正如你可以看到第二个版本更可读。 在简单的形式中， **switch**有两个参数： 

```Tcl
switch string {pattern body pattern body ?pattern body? ...}
```

 第一个参数是我们要针对模式进行测试的字符串，第二个参数是具有匹配对象的*模式*对象的列表，以及脚本的*正文* ，如果该模式匹配则执行。 如果最后一个模式是字符串“default”，则如果没有一个先前的模式匹配，则相对体将被执行。 

 这是开关的主要思想，但实际上命令有点复杂，因为它支持另一种形式，还有一些选项可以改变字符串与模式的匹配方式。 这是命令的完整签名： 

```Tcl
switch string {pattern body pattern body ?pattern body? ...}
```

 第一个参数是我们要针对模式进行测试的字符串，第二个参数是具有匹配对象的*模式*对象的列表，以及脚本的*正文* ，如果该模式匹配则执行。 如果最后一个模式是字符串“default”，则如果没有一个先前的模式匹配，则相对体将被执行。 

 这是开关的主要思想，但实际上命令有点复杂，因为它支持另一种形式，还有一些选项可以改变字符串与模式的匹配方式。 这是命令的完整签名：

```Tcl
switch ?options? string {pattern body ?pattern body ...?}
```

  或者，替代： 

```Tcl
switch ?options? string pattern body ?pattern body ...?
```

 第二个形式只是意味着将*图案体*对传递到列表中，也可以直接作为**开关参数**传递。 写这个很简短的代码的时候，你可能会喜欢这个表单： 

```Tcl
switch $a 1 {return one} 2 {return two} 3 {return three}
```

 但一般来说，从缩进的角度来说，第一种形式更好。 另外需要注意的是， **交换机**可以选择修改他的行为。 支持的选项有： 

```
-exact        Use exact matching when comparing the string with patterns.
-glob         Use glob style matching, like the [string match] command.
-regexp       Use regular expressions for matching.
--            End of options, all the rest is interpreted as an argument
              even if it starts with a - character.
```

 默认匹配类型是*-glob* ，这不是很舒服，因为大多数时候你可能想要匹配匹配的字符串（即使用-exact）。  **switch**选项的另一个问题是，如果*字符串*参数以**-**字符开头，则将其解释为一个选项，除非在命令名后面有一个参数。 所以，大多数时候你想使用这样的东西： 

```
switch -exact -- $string {
    1 {return one}
    2 {return two}
    3 {return three}
    default {return Hello}
}
```

这是使用**switch**的安全方式。 当然，如果您喜欢使用glob风格或正则表达式匹配，您可以用-exact替换所需的内容。 

>  从Tcl 8.5开始， **switch**支持两个新选项，它们是*-matchvar*和*-indexvar* 。 这两个选项都是关于正则表达式匹配样式（ *-regexp*选项），并且在您的Tcl体验的这个阶段并不重要，但如果需要使用与**switch**匹配的regexp，并且要存储在变量中，请务必检查手册页匹配模式的匹配或子匹配。 

 值得注意的是，像**if**命令一样， **switch**返回在匹配的分支中执行的最后一个命令的值，所以例如可以写下如下： 

```Tcl
proc identity x {return $x} ; # The identity function, returns its argument.
set word [switch $number 1 {identity one} 2 {identity two}]
```

 如果`$x`与其中一个模式匹配，则身份函数仅用于返回我们感兴趣的值以分配给*单词*变量。 

## for命令

  Tcl **for**命令以C语言为模型，就像许多其他语言一样。  **for**点复杂。 更常见的用法是在算术进程上运行变量。 

 以下是for循环的例子，用于从0到9： 

```Tcl
for {set i 0} {$i < 10} {incr i} {
    puts $i
}
```

 如果您尝试执行此代码，则输出为 

```Tcl
0
1
2
...
```

 如果你看代码，很容易注意到，将四个Tcl脚本作为参数，这是**for**命令的*签名* ： 

```Tcl
for start test next body
```

  for循环开始评估*起始*脚本，在我们的例子中是Tcl代码“set i 0”。 这将把变量*i*设置为零。  *起始*脚本只评估一次，作为**for**循环的第一步。 现在循环可以启动，遵循以下规则： 

 使用**expr**评估*测试*参数，如果结果为true，则执行*主体*脚本，则执行*下一个*脚本。 现在完成了一个交互，如果*测试*是真的，则循环重启测试。 第一次*测试*为false时， **for**循环终止。 

 该命令始终返回一个空字符串。 请注意，可以将各种Tcl脚本放在*start* ， *next*和*body*参数以及*test*参数中的各种有效的**expr表达式**中。 例如，这是一个无限循环： 

```Tcl
for {} 1 {} {
    puts "I'll print this forever"
}
```

 类似地，可以使用多个变量： 

```Tcl
for {set x 0; set y 0} {$x+$y != 50} {incr x 2; incr y 3} {puts .}
puts "$x+$y = 50"
```

 上述代码将x增加2和y加3，当两者的和将为50（10次迭代后）时，将停止。 在该示例中， *body*参数为空，因为继续计算所需的工作*在下一个*脚本中。 

## break和continue

  Tcl核心命令实现循环结构，如**foreach** ， **while**和**for** ，支持一种从循环过早退出的方法，或者在循环*体*的所有命令执行之前过早重申。 为了控制这个特性，有两个叫做**break**和**continue** Tcl命令。 它们在C编程语言*断开*之后建模，并*继续*关键字。 

  **break**命令是用于过早退出循环的命令。 它可能出现在循环*体*的任何位置。 如果有嵌套循环并且执行了**break**命令，则只有最内部的循环才会终止。 在Tcl **中断**没有任何参数，没有办法指定要退出的级别。 这是使用**break**的代码示例： 

```Tcl

set a 0
while 1 {
    puts $a
    incr a
    if {$a == 10} break
}
```

 程序的输出是从0到9的数字序列。请注意，即使**while**循环apperas成为无限循环（因为条件为真，只有**1** ，永远不会改变）， **break**可以退出如果遇到给定的条件（变量*a*包含10的值），则循环。 

 在循环之外调用**break**命令会产生一个错误，因为没有循环可以转义。 

 您可能会想知道如果break命令只能终止最内层的循环，那么可以退出多个nexring循环。 此示例包含两个嵌套循环： 

```Tcl
set a 0
while 1 {
    puts $a
    incr a
    if {$a == 10} break
}
```

 程序的输出是从0到9的数字序列。请注意，即使**while**循环apperas成为无限循环（因为条件为真，只有**1** ，永远不会改变）， **break**可以退出如果遇到给定的条件（变量*a*包含10的值），则循环。 

 在循环之外调用**break**命令会产生一个错误，因为没有循环可以转义。 

 您可能会想知道如果break命令只能终止最内层的循环，那么可以退出多个nexring循环。 此示例包含两个嵌套循环： 

```
set a 0
while 1 {
    puts $a
    incr a
    if {$a == 10} break
}
```

 为了退出两个级别，我们可以使用以下技巧： 

```Tcl
set dobreak 0
while 1 {
    while 1 {
        set dobreak 1; break
    }
    if {$dobreak} break
}
```

 这次为了退出这两个循环，我们将一个*dobreak*变量*初始化*为零，并在每次退出内部循环时测试它，如果设置为1，则在外部循环中再次调用**break** 。在内部循环中，我们可以使用只要我们想逃脱一个循环，或者`set dobreak 1; break*` 当我们想要逃脱的时候**打破** 。 

 您几乎不需要使用这种技巧，因为需要打破多个循环是不常见的，但如果真的需要，知道如何执行此操作可能是有用的。 

 请注意， **break** 功能与**for** ， **foreach**和其他循环命令的方式完全相同。 

> 如果你是一个C程序员，你可能会记得在C中可能会有一段*时间*或者一个嵌套的内置的循环。 因为在C中， *交换机*需要*中断*才能终止一个*情况，*你不能使用*break*来从*交换机*中逃脱循环。 相反，需要像上面的dobreak一样的标志变量，或者使用*goto* 。 在Tcl中，这个问题并不存在，因为Tcl的**开关**不会以任何方式使用**断点** ，所以在**开关**内部的**断开**将产生终止最内圈的设计效果。

外部循环将继续运行，因为nexted **while**循环的中断只会影响。 

  **continue**命令类似于**break** ，而是终止执行中的最内层循环，它只会重申它。 在这个上下文中指定重要的意义很重要：当遇到**继续**时，如果在循环*体*中不再执行命令，会发生什么情况。 在**while**循环的情况下，将再次评估*测试*条件，如果仍然为true，则循环体将被执行。 在**foreach**的情况下，列表的下一个元素将被分配给foreach变量，并且*主体*将被再次执行，依此类推。 

 以下是**continue**的示例： 

```Tcl
foreach x {0 1 2 3 4 5 6 7 8 9} {
	if {$x < 3} continue
	puts $x
}
```

 上述Tcl程序将从3到9输出数字，因为在每次迭代时，如果**$ x <3**为真，则循环将重复执行，而不执行**puts $ x**命令。 

## 缺乏goto

 在Tcl中没有**goto**命令，有很多理由避免设计高级编程语言，实际上本书的作者在C中有很好的用途，为了处理异常或者逃避嵌套循环，但是当在Tcl中进行编程时，很难错过**goto**命令。  Tcl处于更高级别：有异常处理，内部循环可以使用**break**来终止循环，一般在较高级别工作时，缺少**goto**命令不是问题。 大多数时候，您需要在C中进行**goto** ，您可能希望在Tcl中实现*状态机* 。 

 尽管如此，Tcl的目标是灵活性，在下一章中，您将学习如何在Tcl本身中编写实现新的控制结构的Tcl命令，包括类似于**goto的内容** 。 

 因为知道可以记住，有可能使用**foreach**和**break**来实现可能类似于无条件跳转的东西，如下面的代码： 

```Tcl
foreach _ _ {
  ... some code ...
  if {$condition} break ; # Will exit this "block"
  ... some other code ...
}
```

  **foreach**将在由一个元素（“_”字符串）组成的列表中使用变量*__*来迭代一次。 当**foreach**身体会**break**身体会终止。 

# 扩展TCL

  Tcl是一种可编程的编程语言。 语言的几乎每个功能都通过命令导出到用户：条件，创建过程，控制结构，数学表达式，都被实现为以字符串作为参数的命令。 因为用户可以创建新的命令，甚至可以用用户定义的命令替换核心命令，所以语言可以以一种激进的方式进行扩展和修改。 您可以编写实现自己的控制结构的命令，一种处理数学表达式的新方法，与正在编写的程序密切相关的功能或面向对象的编程扩展。 所以专家Tcl程序员将以一种新的方式写一个程序：在第一阶段，他将专门使Tcl使语言更好地完成任务，然后他将使用这种*新的*专门语言编写程序。 

 我们已经看到了如何创建Tcl程序，但是为了创建更强大的程序，需要更强大的命令，这个命令是**eval** ， **uplevel** ， **upvar** ，还有其他命令：这些命令将在本章中进行说明，关于如何在Tcl中编程Tcl的例子，以增加语言的力量。 

## 执行程序的程序：eval命令

 在本书的第一章中，我们已经表明，Tcl具有很强的动态性。 例如，您可以在变量中放置一个命令名称，并将其命名为： 

```Tcl
% set a llength
llength
% $a {1 2 3}
3
```

 您甚至可以使用插值在运行时创建命令名称（或其任何参数）： 

```Tcl
% [string range "Xllen" 1 end]gth {1 2 3}
3
```

 仍然有一些非常可取的，我们不能使用一个名为**eval**的命令，即将一个字符串作为Tcl程序来评估。 这就是**eval**命令的作用：它**运行**您作为参数传递的Tcl脚本，并使用该脚本中执行的最后一个命令的返回值作为返回值。 这是一个例子，只是为了说明它的工作原理： 

```Tcl
% eval "set foo 10"
10
% set myscript {puts $foo}
puts $foo
% eval $myscript
10
```

```

```

 在第一行，我们使用字符串“set foo 10”作为参数调用**eval** 。  Eval只会评估它，并返回脚本返回的值。 然后我们将变量*myscript*设置为一个表示有效Tcl命令的字符串，并使用**eval**执行它。 这是非常重要的操作，例如，您想要编写一个名为**repeat**的新Tcl控件结构。 目标是避免一个**for**或**while**循环，当你想要重复一个给定的Tcl脚本多次，而是使用“repeat 10 {lappend mylist foo}”来创建一个包含10个值为“foo”的元素的列表“。  **repeat**命令的第二个参数是脚本，根本不可能在循环中执行此脚本，而不使用**eval** 。 这是**repeat** 命令的可能实现： 

```
proc repeat {n script} {
    while {[incr n -1] >= 0} {
        eval $script
    }
}
```

 在*tclsh*中剪切并粘贴，以检查它是否正常工作： 

```Tcl
% repeat 4 {puts Hello}
Hello
Hello
Hello
Hello
```

 太好了，我们只是在Tcl中扩展Tcl。 其实我们稍后会看到，这种**重复**的实现不是很正确，但是为了了解我们的目标是非常了解**eval的**工作原理。 关于**eval的**一个重要信息是它可以使用多个参数，它们将使用**concat**命令进行连接，并且级联的结果将被评估。 所以其实写的是： 

```Tcl
eval $foo $bar
```

 是一样的叫 

```Tcl
eval [concat $foo $bar]
```

**concat**命令执行一个简单的操作：它删除传递的每个参数的右侧和左边的每个空格，然后使用单个空格连接所有生成的字符串作为每个字符串之间的分隔符，将获得的结果返回给调用者。 使用一些示例来显示行为可能更简单： 

```Tcl
% concat {a  } b
a b
% concat a b c
a b c
% concat {1 2 3} {a b c}
1 2 3 a b c
% 
```

 虽然**concat**只对字符串执行一个操作，但您应该注意到，如果其所有参数都是有效列表，则结果是单个列表，即所有原始列表的并置。 所以**concat**可以用来连接列表。 因为**eval**连接了**concat这样的参数** ，所以当参数是脚本时，我们必须非常仔细地研究**concat**行为，以便我们可以应用我们对**concat**的理解，以便很好地使用**eval** 。 

 让我们从一个例子开始，但首先我们需要回顾一下我们写过的**+**程序的最后一个版本，讲的是可变数量的参数的程序。

 ```Tcl
proc + {x args} {
    foreach e $args {
        set x [expr $x+$e]
    }
    return $x
}
 ```

 现在假设你有一个整数列表，并且你想使用**+**过程来总和所有这个整数。  **+**被设计为使每个整数作为一个分离的参数求和，所以我们不能写“+ $ mylist”，但是可以使用**eval**完成工作： 

```Tcl
% set mylist [list 1 2 3 4 5]
1 2 3 4 5
% eval + $mylist
15
```

 它的工作原理，并且行为被解释回忆起， **eval**连接它的参数，如**concat** 。 我们可以使用完全相同的参数来调用**concat**而不是**eval** ，以检查在连接步骤之后**eval**将最终评估的结果脚本： 

```Tcl
% set mylist [list 1 2 3 4 5]
1 2 3 4 5
% concat + $mylist
+ 1 2 3 4 5
```

 结果脚本是“+ 1 2 3 4 5”，它是完美的，因为**+**过程将被调用与每个列表元素作为一个不同的参数。 这个行为是有用的，但是我们也想*避免*这种情况，那就是如果我想使用一个参数，即使它包含空格也应该被视为*一个*单独的参数呢？ 将参数传递给eval将不起作用： 

```Tcl
% set a "puts"
puts
% set b "Hello World"
Hello World
% eval $a $b
can not find channel named "Hello"
while evaluating {eval $a $b}
```

 返回一个错误，因为[concat $ a $ b]将产生“puts Hello World”，所以**put**将使用三个参数调用，而不是像“puts {Hello World}”中的两个。 为了解决问题，我们必须使用**list**命令*引用*参数。 首先我们可以直接尝试看到生成的字符串调用**concat**的区别： 

```Tcl
% set a "puts"
puts
% set b "Hello World"
Hello World
% concat $a $b
puts Hello World
% concat [list $a $b]
puts {Hello World}
```

 正如你所看到的，在最后一行，我们使用**列表**来引用命令：list将关心使用大括号围绕参数，包含Tcl以特殊方式解释的空格或其他字符。 使用或不使用**list**命令来引用参数，使得我们能够选择应该如何*扩展* **eval的**参数，就像使用**+**过程来计算数字列表一样，而不应该是这样。 例如在代码中： 

```Tcl
eval $a [list $b $c $d] $e
```

 如果**$ a**和**$ e**包含空格，则内容将被解释为多于一个参数，而$ b $ c和$ d将始终被解释为单个参数，因为**list**命令引用。 

 不要担心，如果所有这一切现在不是很清楚，这是一个复杂的话题，你会掌握在一些pratice之后。 现在我们来介绍一个新的命令，这将使我们能够编写一个新的更好的**repeat** 。 

## 打破上级规则

 在上一节中，我们写了**repeat**命令的这个实现。 

```Tcl
proc repeat {n script} {
    while {[incr n -1] >= 0} {
        eval $script
    }
}
```

 它在某些情况下起作用，就像我们使用它在屏幕上打印四次“Hello World”一样： 

```Tcl
repeat 4 {puts "Hello World"}
```

 但是如果你看**repeat** ，有一些奇怪的事情。 包含循环体的*脚本*参数在**repeat**过程的上下文中执行 ，而不是在调用者的上下文中执行。 在`{puts“Hello World”}`这样的脚本的情况下，没有问题，因为这个脚本可以在任何上下文中执行，没有麻烦，它不引用局部变量，它只是一个具有常量参数的命令。 相反，这将产生麻烦： 

```Tcl
% set a 10
10
% repeat 2 {puts $a}
can't read "a": no such variable
while evaluating {repeat 2 {puts $a}}
```

 发生的是脚本`puts $ a`在**重复**过程的上下文中**执行** ，其中`$a`变量不存在。 我们需要修复此过程，是一个非常类似于**eval**的命令，但能够**在调用者的上下文中**运行脚本。 这个命令叫做**uplevel** 。  uplevel命令与**eval**完全相同，但它将脚本执行的*级别*作为第一个参数。 这是命令*签名* ： 

```Tcl
uplevel ?level? arg ?arg ...?
```

 例如，如果level为1，则在调用者过程的上下文中，一级执行代码，如果level为2，则在调用方的调用方的上下文中执行代码，依此类推。 仔细查看以下示例： 

```Tcl
proc a {} {
    set myvar "Tcl is a programmable programming language"
    b
}


proc b {} {
    c
}


proc c {} {
   uplevel 2 {puts $myvar}
}


a
```

```

```

 该程序的输出是“Tcl是一种可编程的编程语言”，但重要的是要理解为什么。 程序创建三个过程**a** ， **b**和**c** ，并开始调用过程**a** 。  **一个**过程将使用“Tcl is a ...”字符串作为内容创建一个局部变量*myvar* ，并将调用**b** ，它将调用**c** ，它将在调用者的上下文中执行脚本“puts $ myvar”调用者（即**一个**过程）。 因为在*myvar*变量的上下文中存在并设置为给定值，程序将打印它并退出。 

 现在你需要改变**repeat**过程来使其工作，就是用**eval**来替换**上层1** ，这样就可以了。 

```Tcl
proc repeat {n script} {
    while {[incr n -1] >= 0} {
        uplevel 1 $script
    }
}
```

 该版本将按预期工作：您无法将其与核心命令区分开来，您只需要一个新的控制结构即可在需要时使用。 当最后一次执行失败时，这个新程序当然会起作用： 

```Tcl
% set a 10
10
% repeat 2 {puts $a}
10
10
```

  **repeat **和其他Tcl命令执行循环的唯一区别在于它不适用于**break**和**continue** 。 当然也可以解决这个问题，我们将在关于Tcl的错误处理功能的一章中看到如何做到这一点。 

## 将变量名传递给程序

 现在我们有一个像**上级**的工具，我们可以看出如何实现可以将变量名称作为参数的过程，以及修改或使用调用者上下文中的这个变量的内容。 这是像**incr** ， **append** ， **lappend** ， **foreach**这样的命令的情况。 在某些方面，这是Tcl *通过引用传递的方法* ，但是不是引用传递的是调用者变量的名称。 

 我们尝试实现一个给定变量名的过程，将存储在该变量中的字符串转换为大写，将该字符串的大写版本设置为变量的新内容。 

```Tcl
proc toupper varname {
    set oldval [uplevel 1 [list set $varname]]
    set newval [string toupper $oldval]
    uplevel 1 [list set $varname $newval]
    return {}
}
```

 用法很简单： 

```Tcl
% set myvar "tcl"
tcl
% toupper myvar
% puts $myvar
TCL
```

 请注意， **toupper的实现**返回一个空字符串，以便强调函数作为*副作用*的工作，而不是返回值。 你应该能够理解如何工作，但我们将一步一步地分析它。 

 从第一行程序开始： 

```Tcl
set oldval [uplevel 1 [list set $varname]]
```

 该命令将*oldval*变量设置为在调用者的上下文中执行的脚本`set $ varname`的值。 你还记得只有一个参数的**set**命令返回指定变量的值吗？ 但是这个变量存在于调用者的上下文中，所以我们需要使用**uplevel** ，并且使用**列表**引用脚本，因为`$varname`的值可能包含空格。 

 程序的下一行只是基本的Tcl： 

```Tcl
set newval [string toupper $oldval]
```

 我们使用**string**命令将**$ oldval**的大写版本设置为*newval*变量的值。 在这一点上，我们准备在调用者的上下文中修改名为**$ varname**的变量来存储字符串的新的*upperCased*值： 

```Tcl
uplevel 1 [list set $varname $newval]
```

```

```

 这与程序的第一行非常相似，但是这里**set**是通过三个参数调用（再次，在调用者的上下文中，由于**uplevel** ），最后一个是要分配的新值。 为了确保即使**$ varname**或**$ newval**包含空格也是必须的，它们将被作为一个参数处理。 

 最后，该过程的最后一行返回空字符串。 

 这一切都很好...但是有一个问题：它有点太复杂了要舒适，所以Tcl有一个名为**upvar**的命令，使这更简单。 以下是使用**upvar**而不是**uplevel**的等效程序： 

```Tcl
proc toupper varname {
    upvar 1 $varname var
    set var [string toupper $var]
    return {}
}
```

 这看起来更简单，但它是如何工作的？  **upvar**能够绑定生活在当前过程中的变量名，一个生活在不同的级别。 这是程序*签名* ： 

```Tcl
upvar ?level? otherVar myVar ?otherVar myVar ...?
```

  *level*参数的作用与*上级的level*参数完全一样，所以level值为1表示绑定在调用者的上下文中的一个变量。 在上面的例子中，代码： 

```Tcl
upvar 1 $varname var
```

 意思是：绑定局部变量名*var* ，调用者的变量名为**$ varname** 。 每次**toupper**过程将使用变量*var* ，效果将访问调用者的变量名为**$ varname** （当然，varname的实际值取决于传递给**toupper**过程的参数的值）。 

 可以为**upvar**指定多个变量名称，如： 

```Tcl
upvar 1 $listname list $somename foo
```

 以便使用单个**upvar**命令绑定更多变量。 

## 将脚本映射到列表

 现在我们可以扩展语言，我们可以编写一个程序，在许多情况下证明是非常有用的。 这个称为**map的**过程用于将脚本映射到Tcl列表。 列表的每个元素都用作Tcl脚本的参数，结果用于生成新的列表（由生成的元素组成）。 例如，将脚本返回到“1 2 3 4”列表中，我们将获得列表“1 4 9 25”，依此类推。 以下是**map**的第一个实现，但是我们将在关于*Tcl的功能编程*的章节中写一个更好的例子。 

```Tcl
proc map {varname mylist body} {
    upvar 1 $varname var
    set res {}
    foreach var $mylist {
        lappend res [uplevel 1 $body]
    }
    return $res
}
```

 该**map**过程与**foreach**命令非常相似，但是在每次迭代时，脚本的返回值都将附加到循环结束时由**map**返回的列表。 例如，为了计算您可以写的前5个自然数的平方： 

```Tcl
map x {1 2 3 4 5} {expr $x*$x}
```

 该命令的返回值为列表{1 4 9 16 25}。 另一个例子是使用map来转换其长度列表中的字符串列表。 这次*tclsh*用于交互式会话： 

```Tcl
% set l {I will be translated into a list of length} 
I will be translated into a list of length
% map x $l {string length $x}
1 4 2 10 4 1 4 2 6
```

 它使很多任务更简单！ 实际上， **map**做的是在一个程序中封装一段代码，否则可以输入多次，这就是这样的： 

```Tcl
set result {}
foreach e $list {
    lappend result [string length $e]
}
```

 这解释了为什么有经验的程序员认为具有极大扩展能力的编程语言更好：程序员可以在代码中查找模式（程序员正在做的重复操作的症状，语言可能为他做），并编写一个程序使特定任务发生频繁更简单。 

## rename命令

 在第一章中，我们提到使用**proc**可以创建具有已有命令（包括核心命令或已定义的命令）的名称的过程。 不仅可以*覆盖*现有的命令名称，还可以使用**rename**命令重命名现有命令。 用法非常简单： 

```Tcl
rename oldName newName
```

 重命名将命名*oldName*的名称更改为新名称*newName* ，但有一个例外：如果*newName*参数为空字符串，则*oldName*命令将从Tcl中删除。 可以使用此功能，以使语言在关键上下文中更安全（删除所有有潜在安全问题的命令），但我们将在*下一章*中看到更好的*安全解释器* 。 

  **rename**命令进入本章跟踪的图片，因为重命名命令的能力导致*包装*现有命令的能力。 例如，您可能希望包装**puts**命令，以便在使用多个参数调用时充当**eval**命令，使用空格分隔所有参数并将结果打印在屏幕上，但同时我们需要把原来的**put**实现放在别的名下，因为我们的new **put**需要调用它才能执行输出。 我们可以使用**重命名**做所有这些： 

```Tcl
rename puts _puts
proc puts args {
	_puts [join $args]
}
```

 执行上述代码之后，put将使用多个参数： 

```Tcl
% puts multiple arguments passed to puts
multiple arguments passed to puts
```

 包装过程可能很有趣，例如，您可能希望扩展**proc**功能，以便将**proc**描述定义为附加参数，然后编写命令以从命令名称获取文档，或者按顺序包装**set**在日志文件中打印关于修改变量的调试信息，以便跟踪一些错误，等等。 

## Tcl 8.5中的列表扩展为参数

 谈到**eval，**我们展示了如何使用这个命令来执行列表的元素作为单个命令参数的扩展。 在这个例子中，我们使用这个特征和**+**过程来计算整数列表的总和： 

```Tcl
% set mylist [list 1 2 3 4 5]
1 2 3 4 5
% eval + $mylist
15
```

 从Tcl 8.5开始，为了执行*扩展，*有一种更好的方式（从性能上和从用户的角度来看）。 这不是一个命令，而是新的语法。 基本上使用字符串{expand}添加一个参数的结果是，Tcl解释器将把原来参数的每个元素都解释为一个Tcl列表，将其扩展为一个参数。 例如脚本： 

```Tcl
{expand}{puts Hello}
```

 和 

```Tcl
set mylist [list puts Hello]
{expand}$mylist
```

 都相当于 

```Tcl
puts Hello
```

 我们可以使用{expand}重写获取列表的整数元素的总和，而不使用**eval** ： 

```Tcl
+ {expand}$mylist
```

  **$ mylist的**每个元素都将被扩展为**+**命令的参数。

> 在Tcl语法中引入{expand}是有争议的，许多人不喜欢语法，或者使Tcl语法更加复杂的想法（幸运的是，简单的想法在Tcl的团队中非常重要）。 严格来说，可以编写各种程序，而不必使用{expand}，但实际上有很多情况需要扩展参数， **eval**不是参数扩展的最佳工具（ **eval**的主要目标是不同的，它是评估Tcl脚本）。 我相信，在*Tcl核心团队*决定将{expand}语言引入到语言中是一件很好的事情：现在，Tcl 8.5接近发布，Tcl的朋友们开始欣赏如何舒适和快速的论证扩展可以。