---
title: Linux shell的演进史
url: https://www.ttalk.im/articles/UkLrWx
source: The Talk
date: 2025-12-26
fetch_date: 2025-12-27T03:24:47.960122
---

# Linux shell的演进史

[The Talk](/)

![ Linux shell的演进史](/files/snDrTH/9cf76a27558b409d)

# Linux shell的演进史

[![david](/files/snDrTH/5a4d41de51604174)david](/u/snDrTH)●

2025年12月26日

●

28 次阅读

[杂谈](/u/snDrTH?tag=Ejh8xT)

深入解析Linux Shell演进史：从1971年Ken Thompson开发的V6 Shell到现代Bash、Zsh的发展历程，涵盖Bourne Shell、C Shell、Korn Shell的技术特性与脚本对比。

Shell 与编辑器有个类似之处：每个人都有自己心仪的一款，而且非常排斥别的选择（还会告诉别人为什么应该换掉）。确实，不同的 shell 能提供不同的特性，不过它们都实现了数十年前发展出的核心理念。 我是在80年代第一次接触现代shell的，当时我正在SunOS上进行软件开发。当了解到能够把一个程序的输出作为另一个程序的输入（甚至能用链式方法重复多次），因此我能轻易且高效地实现过滤器和转化。利用这一核心特性，就能够开发出简单灵活的工具，通过与其它工具进行整合起到更大的作用。从这点看，shell不仅提供了与内核和设备交互的方法，还集成了一些今天软件开发中通用的设计模式（比如管道和过滤器）。 让我们从现代 shell 的一段简要历史开始，探索如今 Linux 上的一些实用且新奇的 shell。

## Shell的一段历史

Shell，或者称命令行解析器，有一段漫长的历史，不过这里的讨论从第一个 UNIX® shell开始。Ken Thompson（贝尔实验室的那位,C和Unix之父）在1971年为UNIX开发了第一个shell，称为V6 shell。与其Multics上的祖先类似，这个shell (/bin/sh) 是运行在内核之外，完全独立的用户程序。像通配符（用于参数展开的模式匹配，如 \*.txt）这种功能是在一个叫glob实用程序完成，执行条件表达式的if也是如此。这种分拆式的设计使得shell本身很小，C语言源码不超过900行。

现代的shell还引入了很多更加简洁的重定向语法(< > 和 >>)以及管道语法(| 或 ^)。同时我们也可以发现我们现在能顺序的执行一组命令（需要使用`;`进行分割）也可以异步的执行命令（需要使用`&`）。

同时Thompson所开发的shell并不具备执行脚本的能力。它唯一的目的就是作为交互shell（解析命令）来执行各种各样的命令并查看结果。

> shell作为一种小型语言
>
> 我们可以认为shell是为了满足的用户与操作系统进行交互的这一场景而开发的一种特化的领域专用语言（DSL，小型语言）。并且，我们可以找到使用字符模式的系统shell，带有图形界面的shell或者某些语言的shell（例如Python的shell和Ruby的irb）。而shell的设计思路也影响到了一个叫做[goosh](https://goosh.org?utm_source=ttalk.im&utm_medium=website&utm_campaign=Tech%2520Talk)的搜索引擎网站。这个shell通过命令行中命令，如search，more和go搜索Google。

## 1977年后的UNIX shell

在Thompson所实现的shell之后，我们来看看1977年出现的现代化shell -- Bourne shell。Bourne shell是Stephen Bourne在AT&T贝尔实验室为V7 UNIX所开发的，并且作为非常实用的shell一直沿用至今（在某些情况下，bash是系统默认shell）。作者在继ALGOL68编译器之后开发了Bourne shell，所以你会发现它的语法比起其他shell更像ALGOL。即使源码本身用了C语言，还是利用宏增添了一种ALGOL68的语言风格。

Bourne shell有两个主要的目标：作为一个命令解析器去交互式地为操作系统执行命令和脚本化编程（编写可重用的shell脚本）。除了替代Thompson shell，Bourne shell还有几个优点，把控制流程，循环，变量引入了脚本，为系统操作（不管是交互环境，还是非交互环境）提供了一种功能性更强的语言。Bourne shell还允许我们把shell脚本作为过滤器，同时也提供了对信号处理的支持，但缺少函数定义的功能。最后，它集成了许多今天使用的特性，包括命令替换（使用反引号）和在脚本内嵌入原始字符串的HERE文档。

Bourne shell不仅是一个重要的进步，现在，那些主流Linux 系统使用的shell，许多都以它为基准。图1展示了一些主要的shell的家族关系。Bourne shell导致了 Korn shell(ksh)，Almquist shell(ash)，Bourne Again Shell(bash)的诞生。C shell(csh)在Bourne shell发布时处于开发阶段。图1展示了主要的谱系，但并非全部影响，有些未提及的shell也起到了重要作用。

![linux-shell-figure1.gif](/files/snDrTH/9cf76a27558b409d)

[图1.1977年后的UNIX shell

我们会在后面探讨其中的一些shell，看它们的语言和特性的例子是如何影响他们进化的。

## Shell 基本架构

一个假想的shell的基础架构是很简单的（Bourne shell已经证实了这件事）。从图2可以看出，这个架构类似一个流水线，在里面进行输入分析和解析，符号进行展开（使用各种方法，比如大括号 {} 、波浪符 ~ 、变量和参数的展开/替换、文件名展开），并最终执行命令（通过 shell 内置命令或外部命令）。

![linux-shell-figure2.gif](/files/snDrTH/5811c56b10074735)

图2. 假想的shell的简单架构

## 探索 Linux shell

现在让我们来探索一些shell，在每个小节看一个脚本例子，回顾它们对shell发展所做出的贡献。此处我们将回顾C shell，Korn shell和Bash。

### Tenex C shel

C shell是加州大学伯克利分校的一名研究生Bill Joy（译注：Bill Joy真业界大神，随便搞搞写的TCP实现都比那些研究员搞的好，独立设计了Sparc芯片，还非常善于搞操作系统）在1978年为伯克利UNIX(BSD UNIX)开发的。五年后，C shell被引入了Tenex系统（流行于DEC PDP上系统）上的一些功能。Tenex增加了命令行编辑特性，还带来了文件名和命令的自动补全。Tenex C shell(tcsh\*\*向后兼容了csh并全面提升了它的交互特性。tcsh是由在卡内基梅隆大学的Ken Greer所开发。

C shell的设计中一个核心目标是开发一个与C语言类似的脚本语言。这是一个很实用且有价值的目标（译注：在过去，计算机的使用者绝大部分都是程序员自身，尤其是UNIX类的操作系统，并且当时C语言算是为数不多的高级语言了，如果能脚本化，但让是会广受好评），因为C语言使用广泛（而且很多操作系统是C语言开发的）。

命令历史是Bill Joy在C shell引入的一个非常实用的特性。这一特性保留了之前执行过的命令，用户能够查看并快速选择历史命令去执行。比如，输入命令history会展示之前执行的命令，使用上下方向键选择历史命令，上一条命令能通过`!!`执行。甚至可以指代上一条命令的参数，比如`!*`代表上一个命令的所有参数，`!$`代表上一个命令的最后一个参数。

让我看一个简单的tcsh的脚本（清单1）。这个脚本有一个参数（一个目录名），它会找出该目录下所有可执行文件的文件名和数量。后面我将在所有的例子中使用这个目的来编写脚本，用来展示shell之间的彼此不同。

tcsh脚本被分为3个不同部分。第一部分，是shebang，用来表示该文件将被shell来执行（此处，就是tcsh的可执行文件）。这样可以让我们像执行可执行文件一样来执行这个文件，而不是在这个文件前面添加它所需要的解释器的名字。因为脚本需要对可执行文件数量进行记录，因此我需要初始化一个技术器`count`，并将其设置为0。

**清单1： tcsh寻找所有的可执行文件**

```
#!/bin/tcsh
#find all executables

set count=0

#Test arguments
if ($#argv != 1) then
  echo "Usage is $0 <dir>"
  exit 1
endif

#Ensure argument is a directory
if (! ‑d  $1) then
  echo "$1 is not a directory."
  exit 1
endif

#Iterate the directory, emit executable files
foreach filename ($1/∗)
  if (‑x $filename) then
    echo $filename
    @ count = $count + 1
  endif
end

echo
echo "$count executable files found."

exit 0
```

脚本第一部分用于验证用户传递的参数。`#argv`变量指传递的参数数量（不包括命令名称）。你能通过下标获取到这些参数，比如，`#1`指第一个参数（`argv[1]`的缩写）。脚本期望接受一个参数，如果没有找到，它会报出一个错误消息，错误消息中使用了`$0`指代控制台输入的命令名称（`argv[0]`）。

第二部分确保传递的参数是一个目录。`-d`运算符会在参数是目录的时候返回 true。不过，注意我前面使用了逻辑非运算符 `!`。这样，整个表达式会在参数不是一个目录时打印错误消息。

最后一部分遍历了目录内的所有文件，判断它们是否可执行。我使用了便利的`foreach`迭代器，它会循环遍历括号内（这里是目录）的所有条目，然后作为循环体的一部分执行。这个步骤使用了`-x`操作符来判断文件是否可执行，如果是，则输出文件，并增加计数。脚本最后输出了可执行文件的数量。

## Korn shell

David Korn设计开发的Korn shell(ksh)几乎与Tenex C shell同时出现。ksh引入了一个重要的特性，在向后兼容原来的Bourne shell的同时，还能作为脚本语言使用。

Korn shell之前一直是专有软件，直到2000年才开源（以Common Pulic License协议开源）。除了对Bourne shell的良好兼容，Korn shell也采用了其它shell的特性（如csh的历史功能）。Korn shell还提供了几个与现代脚本语言Ruby和Python类似的高级功能。比如，关联数组和浮点运算。Korn shell 支持很多操作系统，包括IBM®，AIX®和HP-UX®，并且力求支持Unix可移植操作系统接口（POSIX）的shell语言标准。

Korn shell是 Bourne shell的衍生产品，比起C shell，它与Bourne shell和Bash更像。让我们看一个用Korn shell查找可执行文件的例子（清单2）。

**清单2： ksh查找所有可执行文件的例子**

```
#!/usr/bin/ksh
#find all executables

count=0

#Test arguments
if [ $#‑ne 1 ] ; then
  echo "Usage is $0 <dir>"
  exit 1
fi

#Ensure argument is a directory
if [ ! ‑d  "$1" ] ; then
  echo "$1 is not a directory."
  exit 1
fi

#Iterate the directory, emit executable files
for filename in "$1"/∗
do
  if [ ‑x "$filename" ] ; then
    echo $filename
    count=$((count+1))
  fi
done

echo
echo "$count executable files found."

exit 0
```

你一眼就会发现清单2中这段代码跟清单1中的例子很像。结构上，它们几乎完全一样，关键区别在条件判断、表达式、迭代执行的写法。ksh采用了Bourne风格的运算符（如-eq，-ne和-lt等等），而不是C语言风格的运算符。

Korn shell 在迭代方面也有不同。在Korn shell中，使用了`for...in`结构，通过命令替换的方式，获取命令 `ls $1/*`输出到标准输出的文件列表。

除了上面所所的特性外，Korn shell支持别名（alias，就是用一个用户定义的字符串替换原有命令）。同时Korn shell还有一些特性默认是被关闭的（例如文件名补全）但是可以被用户设置为打开可用。

### Bourne-Again Shell

Bourne-Again shell, 也叫 Bash，是开源项目GNU为了取代Bourne shell而开发的。Bash由Brian Fox开发，并成为了使用最广泛，平台支持性最好（Linux、Darwin、Windows®、Cygwin、Novell和Haiku等）的shell之一。就像它的名字一样，它是Bourne shell的超集，可以直接执行大部分 Bourne shell 脚本。 Bash 在兼容Bourne shell脚本编程的同时，集成了Korn shell和C shell的功能，包括命令历史，命令行编辑，目录堆栈（pushd和popd），一些实用环境变量，命令自动补全等。

Bash在持续发展中引入了一些新功能，包括正则表达式（类似 Perl）和关联数组。尽管其它脚本语言可能不支持这些特性，但可以通过其他方式兼容。正因为这点，清单3中的范例脚本中，除了shebang（/bin/bash）不同外，其他部分与Korn shell中的例子一模一样。

**清单3： Bash找出所有可执行文件**

```
#!/bin/bash
#find all executables

count=0

#Test arguments
if [ $#‑ne 1 ] ; then
  echo "Usage is $0 <dir>"
  exit 1
fi

#Ensure argument is a directory
if [ ! ‑d  "$1" ] ; then
  echo "$1 is not a directory."
  exit 1
fi

#Iterate the directory, emit executable files
for filename in "$1"/∗
do
  if [ ‑x "$filename" ] ; then
    echo $filename
    count=$((count+1))
  fi
done

echo
echo "$count executable files found."

exit 0
```

这些shell的一个关键不同点是它们的软件发布协议。GNU项目的Bash，如你所猜测的，是以GPL协议发布的。但csh、tcsh、zsh、ash和scsh都是以BSD或类BSD协议发布的。Korn shell则属于Common Public License协议。

### 一些新奇的shell

对一些喜欢尝鲜的人，你可以自己的喜好选择可替代的 shell。Scheme shell(scsh)提供了一套Scheme语言（Lisp语言的一个分支）脚本环境。Pyshell尝试使用Python语言来实现类似的脚本功能。最后，在嵌入式系统上，有 BusyBox，它把shell和所有的命令集成到一个二进制执行文件里，以此来简化它的发行和管理。

清单4将使用Scheme shell(scsh)来实现我们的`find-all-executables`脚本。这个脚本看起来有点陌生，但它在功能上与上面的脚本例子是一样的。这个脚本含有三个函数，和一段用于判断参数数量的直接运行代码（在代码的最尾端）。这个脚本的主要部分在`showfiles`函数内，它遍历了一个数组（在`with-cwd`命令后面），对数组内每一个元素，调用`write-ln` 。这个数组是遍历目标目录并过滤出其中的可执行文件后获得的。

**清单4： scsh找出所有的可执行文件**

```
#!/usr/bin/scsh ‑s
!#

(define argc
        (length command‑line‑arguments))

(define (write‑ln x)
        (display x))

(define (showfiles dir)
        (for‑each write‑ln
                (with‑cwd dir
                        (filter file‑executable? (directory‑files "." #t)))))

(if (not (= argc 1))
        (write‑ln "Usage is fae.scsh dir")
        (showfiles (argv 1)))
```

## 结论

很多来自早期的shell的思想和接口保留了将近35年，这些都是那些作者努力的证明。shell在不断地刷新和进化，不过没有什么本质上的改变。尽管不断有新的尝试去创造一些特别的shell，但Bourne shell及其继承者则一直都是使用最广泛的。

## 译著

因为译者本身使用FreeBSD（tcsh）和OpenBSD（ksh）比较多，但是在编译GNU的产品的时候，就需要使用GNU的很多东西，有些时候bash也是必要的依赖。因此对Shell的一些进化和差异感到好奇，因此找到了这篇文章，虽然有一些陈旧，但确实也将shell的一些历史讲出来了，解释了译者内心中的一些疑惑。但是本文中并没有介绍[zsh](https://www.zsh.org/?utm_source=ttalk.im&utm_medium=website&utm_campaign=Tech%2520Talk)有一些可惜，但是也是可以理解的，zsh的功能非常强大，但是zsh也同样很难驾驭。正如文章最后的结论，现在依然有很...