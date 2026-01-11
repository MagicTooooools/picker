---
title: 《征服C指针》
url: https://www.kawabangga.com/posts/7160
source: 卡瓦邦噶！
date: 2026-01-10
fetch_date: 2026-01-11T03:45:05.496025
---

# 《征服C指针》

顶部菜单

* [所有文章](https://www.kawabangga.com/all-posts)
* [演讲](https://www.kawabangga.com/talks)

[卡瓦邦噶！](https://www.kawabangga.com/ "卡瓦邦噶！")

无法自制的人得不到自由。

[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/twitter.png)](https://twitter.com/laixintao)[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/rss-feed.png)](https://www.kawabangga.com/feed)[![](https://www.kawabangga.com/wp-content/themes/heatmap-adaptive/images/rss-comments.png)](https://www.kawabangga.com/comments/feed)

topics

* [Vim系列教程](https://www.kawabangga.com/vim%E7%B3%BB%E5%88%97)
* [如何学Python？](https://www.kawabangga.com/how-to-learn-python)
* [珍藏资料](https://www.kawabangga.com/collection)
* [DB资料集](https://www.kawabangga.com/db)
* [计算机网络实用技术](https://www.kawabangga.com/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E5%AE%9E%E7%94%A8%E6%8A%80%E6%9C%AF)

# [《征服C指针》](https://www.kawabangga.com/posts/7160 "Permalink to 《征服C指针》")

Posted on [2026年1月10日](https://www.kawabangga.com/posts/7160 "12:15") by [laixintao](https://www.kawabangga.com/posts/author/laixintao "View all posts by laixintao") [Leave a comment](https://www.kawabangga.com/posts/7160#respond)

最近读了《征服C指针》，是一本好书。日本人写的计算机技术书籍像娓娓道来的技术博客那样，假设读者没有（或者有很少）相关的背景知识，围绕一个主题，除了介绍干巴巴的知识，还会谈自己的经验和理解，对其他的书和论点做点评，指出一些常见的谬误，这样读者可以更容易理解书里讲的东西。如果只有干巴巴的知识，那么去读维基百科就好了。但是维基百科（无论中文还是英文）的说明一般晦涩难懂，因为百科的首要目标是准确，消除歧义，而不是易于理解。而好的技术书里面会旁征博引，运用举例和比喻，用多的篇幅详细说明难懂的部分，加上作者的经验让读者知道哪些内容是经常使用的，哪些是晦涩又不常用的，哪些是可以辅以技巧理解的。这是我理解的优秀的技术书籍。比如，《流畅的 Python》就是这样一本好书，这本书不光介绍 Python 的编程知识，还有对 Python 语言设计的点评，和其他编程语言的对比，作者自己的理解和评价，读完之后对于整个编程概念的理解都会有提升。

这本书其实就不只是 C 语言的指针，还设计内存分配，C 的语法和编译器，CPU 等知识。从初学者到老手都可以从中有所收获。

另外书中对于有些概念可以给出确定的定义，而不是给出模棱两可的答案。如果永远模棱两可，那么永远都不会错，但是这种内容读起来也是浪费时间。一位得高望重的星际2游戏解说曾经说过：「专业解说*要敢于下判断*。」

以下是一些笔记。每一段引用都是书中的原文，引用如果不是连续的，就来自于不同的段落。非引用的格式是我的评论。

> 在C语言中，记录指针指向何种类型是只到编译器为止的，到了运行的时候就已经没有相关信息了。在运行时，指针的值就只是单纯的地址而已。​“要从这个地址里取出哪种类型的值”这一信息只残留在编译器生成的机器码中。无论是在指针的值中，还是在指针指向的变量的内存空间中，都没有类型的信息。因此，如果把指向int的指针转换成了void\*，就不可能再知道它原来是指向 int 的了。

指针的类型，主要是为了告诉编译器信息。

C

int hoge = 5;
void \*hoge\_p;
hoge\_p = &hoge; <-- 不会报错
printf("%d\n", \*hoge\_p); /\* 输出hoge\_p所指向的内容 \*/

|  |  |
| --- | --- |
| 1  2  3  4  5 | int hoge = 5;  void \*hoge\_p;    hoge\_p = &hoge;  <-- 不会报错  printf("%d\n", \*hoge\_p); /\* 输出hoge\_p所指向的内容 \*/ |

会报错如下：

C

warning: dereferencing `void \*' pointer
error: invalid use of void expression

|  |  |
| --- | --- |
| 1  2 | warning: dereferencing `void \*' pointer  error: invalid use of void expression |

只告知了内存上的地址，却没有告知那里保存的是什么类型的数据，当然无法读取。

改成下面这样可以运行：

C

5: printf("%d\n", \*(int\*)hoge\_p); /\* 将hoge\_p转换成int \*/

|  |  |
| --- | --- |
| 1 | 5: printf("%d\n", \*(int\*)hoge\_p); /\* 将hoge\_p转换成int \*/ |

> 这正是指针运算的特征。在 C 语言中，对指针加1 后，其地址就增加该指针所指向的类型的长度。示例程序中的 hoge\_p 是指向 int 的指针，而在我的环境中 int 的长度是 4，所以对地址来说，加 1 就是前进 4 字节，加 3 就是前进 12 字节。

对指针+1，地址移动的单位是「指针所指向的类型的长度」，这个也是编译器计算的，这是指针需要类型的主要原因。

> 由于空指针可以确保与任何非空指针进行比较都不相等，所以经常作为返回指针的函数发生异常时的返回值使用。

> 例如我工作的地方位于日本名古屋市某栋大楼的 5 楼，某人爬一层楼需要 10秒，那么从地面上到 5 楼需要花费多少秒？50 秒？很遗憾，正确答案是 40秒。想必大家在中学都学过等差数列，等差数列的第 n 项等于“首项 + 公差× (n –1)”​。每个都要减 1，真麻烦……此外，​“1900 年代”并不是 19 世纪，它的一大半属于 20 世纪。更加复杂的情况是，2000 年不属于 21 世纪，而属于 20 世纪。这些问题分别可通过以下方式回避。把大楼里与地面等高的那层计作第 0 层把数列的首项计作第 0 项把最初的世纪计作 0 世纪，把公历最初的年份计作 0 年这种“差 1 错误”的问题在编程中经常发生。因此，普遍认为在一般情况下如果以 0 为基准编号，那么通常（并不是所有）能回避这类问题。

延伸阅读：[为什么要“包含头不包含尾”？](https://www.kawabangga.com/posts/2393)

> 要点
>
> p[i] 是 \*(p + i) 的简便写法。
>
> 下标运算符 [​] 只有它原本的意义，与数组毫无关系。

> 要点
>
> 【比上面的要点更重要的要点】
>
> 但是，千万别写成那样。

> 在 get\_word() 中使用下标运算符访问 buf 的内容，会让人觉得从 main() 传递过来的就是buf 数组。然而，这是个错觉，从 main() 传递过来的说到底只是指向 buf 的初始元素的指针。

函数传递只能传递指针而不能传递数组。即使数组和指针不同，但是在函数传递的时候，数组也会转换为指针，指向数组开头的元素。

> 在 C 语言中，参数全部都是通过值传递的。

> 即便是像全局变量那样在函数外部定义的变量，一旦加上 static，其作用域就只限定在当前源文件内。指定为 static 的变量（函数）对于其他源文件是不可见的（函数也是一样的）​。

> 另外，对于函数（非 static 限定）和全局变量，只要名称相同，即便位于不同的源文件中，也会被当作相同的对象处理。

> 因此，根据操作系统及CPU的不同，需要规定不同的调用方法，这就叫作调用约定（calling convention）​。本书中说明的调用方法是在x86系列处理器中被称为cdecl的调用约定。该方法中所有的参数都通过压栈的方式进行传递。

> malloc() 会遍历链表，搜寻空块，若该块大小足够，就将其分割出来，做成使用中的块，并向应用程序返回紧邻管理区域的下一个地址。free() 会改写管理区域的标志，将该块置为空块，如果上下有空块，就顺便将它们合并成一个块*。这是为了防止块碎片化。* 这种操作称为 coalescing。当没有足够大的空块满足 malloc() 的要求时，就向操作系统请求（在 UNIX 中需要通过 brk()系统调用）扩充内存空间。

> 补充
>
> Valgrind正如前面多次提到的那样，与动态内存分配相关的Bug往往出现在距离它被发现的位置很远的地方，因此调试非常困难。在Linux上可以使用Valgrind工具追踪这类Bug。Valgrind工具用于检测对malloc()分配的内存空间越界读写、忘记free()（内存泄漏）或者对同一块内存空间多次free()*这类问题。*
>
> 如果运气好，标准库 glibc 也可以为我们检测出这个问题。

> 调用 malloc() 之后必定写上相应的free() 是一种谨慎的编程风格。程序员就应该小心翼翼地将 malloc()和 free() 对应起来。“因为调用了 exit()，所以就没必要free() 了”的想法是不负责任的偷工减料行为，是不良的编程风格。不管怎么说，程序员也是人，人就是这么一种在可能犯错的地方必定会犯错的生物。可是，​“必须 free() 派”却偏要大肆宣扬无论如何都要“谨慎地”编码，这种论调其实是于事无补的。
>
> 我认为，​“谨慎地”编码并没有什么了不起的，那些能够尽可能地回避“麻烦事”的人才是优秀的程序员。在我心中，理想的程序员是下面这样的：在能够安全地偷懒的地方尽可能地偷懒，并且尽可能地依靠工具而不是肉眼来进行检查，但在无论如何都需要人工处理麻烦的事情时，会在心中坚定地起誓“总有一天要将它自动化”​。

这是我最喜欢的一段话。Python 的初学者写的代码，会在所有的函数入口都写上 try-catch，称之为防御性编程。我觉得这么做的人肯定会有这样的疑惑：「怎么这么麻烦？」对于这个疑惑，有两类人，一类是认为「这么做一定有道理，作为程序员我们要不辞劳苦地做好工作。」另一类人认为「一定有更方便的方式」。

> 正如图 2-17 所示，填充有时会被放到结构体的末尾。因为在创建结构体数组时，填充是必要的。在将 sizeof 运算符应用到这样的结构体上时，返回的是包含末尾填充部分大小的长度。将结果和元素个数相乘，就可以获取数组整体的长度。

——有关结构体的对齐

> 小端与大端到底哪一种更好呢？这个话题经常引起人们的争论，此处就不再深入讨论了。它们各有各的优点。人类在用纸和笔做加法时也会从低位开始相加，所以对 CPU 来说，或许采用小端的方式更轻松一些，而在人类看来，大端的方式或许更容易理解。

[Little Endian vs Big Endian](https://www.kawabangga.com/posts/6816)

> C 语言的声明要用英语阅读
>
> 我们可以遵循以下步骤解释 C 语言声明。
>
> 先看标识符（变量名或函数名）​。
>
> 从贴近标识符的地方开始，按照如下优先级解释派生类型（指针、数组、函数）​：
>
> ①用于整合声明的括号；
>
> ②表示数组的 [​]、表示函数的 ()；
>
> ③表示指针的 \*。
>
> 完成对派生类型的解释之后，通过 of、to 或returning 连接句子。添加类型修饰符（位于左侧，比如 int、double）​。如果不擅长英语，可以用中文解释。

能正确地阅读 C 指针的声明，函数参数中有关指针的声明，已经 `sizeof` 中的声明，是读此书最大的收获了。

> 像这样可以确定长度的类型，在标准中被称为对象类型（object type）​。然而，函数类型不是对象类型。C 语言中不存在函数类型的变量，因而我们无法（也没必要）确定其长度。我们说过，数组类型是由若干个派生源类型排列而成的类型。因此，数组类型的总长度为：
>
> 派生源类型的长度×数组的元素个数
>
> 但是，由于函数类型的长度无法确定，所以也就无法从函数类型派生出数组类型。也就是说，无法创造出“函数的数组”这种类型。但是，可以生成“指向函数的指针”这一类型。只是指向函数类型的指针是不能进行指针运算的，因为我们无法确定指针指向的类型的长度。

> 当表达式代表的是某处的存储空间时，该表达式就称为左值。与此相对，当表达式仅代表值时，该表达式称为右值。表达式中有时存在左值，有时不存在左值。例如，变量名是左值，而 5 这样的常量、1 +hoge 这样使用运算符的表达式就不是左值。

> 当作为 sizeof 运算符的操作数时
>
> 在以“sizeof 表达式”的形式使用 sizeof 运算符时，由于这里的操作数是表达式，所以即使是对数组使用 sizeof，数组也会被解读为指针，从而只能获取指针的长度——或许有人是这样认为的，但其实在数组作为 sizeof 运算符的操作数的情况下，将数组解读为指针这一规则是无效的，在这种情况下返回的是数组整体的长度。

> 总之，关于指向函数的指针的 C 语言的语法是比较混乱的。造成这种混乱的罪魁祸首就是“函数在表达式中会被解读为指向函数的指针”这一意图不明（难不成是为了与数组保持一致？​）的规则。

在表达式中，数组会被解读为指向该数组初始元素的指针，因此代码可以写成下面这样。

int \*p;
int array[10];
p = array; <-- 将指向array[0]的指针赋给p

|  |  |
| --- | --- |
| 1  2  3 | int \*p;  int array[10];  p = array; <-- 将指向array[0]的指针赋给p |

但是，反过来写成下面这样就不行。`array = p;`

数组和指针截然不同。

> “不要误会我对 goto 语句持有任何教条主义的执念。我只是担忧，很多人把这件事给神化了，甚至认为仅凭某个编程技巧或某个简单的编程原则，就能解决编程语言的概念问题！”

---

Categories: [读书有感](https://www.kawabangga.com/posts/category/books)

Tags: [cdecl](https://www.kawabangga.com/posts/tag/cdecl), [C指针](https://www.kawabangga.com/posts/tag/c%E6%8C%87%E9%92%88), [C语言](https://www.kawabangga.com/posts/tag/c%E8%AF%AD%E8%A8%80), [C语言声明解析](https://www.kawabangga.com/posts/tag/c%E8%AF%AD%E8%A8%80%E5%A3%B0%E6%98%8E%E8%A7%A3%E6%9E%90), [free](https://www.kawabangga.com/posts/tag/free), [goto语句](https://www.kawabangga.com/posts/tag/goto%E8%AF%AD%E5%8F%A5), [malloc](https://www.kawabangga.com/posts/tag/malloc), [sizeof](https://www.kawabangga.com/posts/tag/sizeof), [static作用域](https://www.kawabangga.com/posts/tag/static%E4%BD%9C%E7%94%A8%E5%9F%9F), [Valgrind](https://www.kawabangga.com/posts/tag/valgrind), [void指针](https://www.kawabangga.com/posts/tag/void%E6%8C%87%E9%92%88), [值传递](https://www.kawabangga.com/posts/tag/%E5%80%BC%E4%BC%A0%E9%80%92), [全局变量](https://www.kawabangga.com/posts/tag/%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F), [内存分配](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D), [内存合并](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E5%90%88%E5%B9%B6), [内存填充](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E5%A1%AB%E5%85%85), [内存模型](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B), [内存泄漏](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F), [内存碎片](https://www.kawabangga.com/posts/tag/%E5%86%85%E5%AD%98%E7%A2%8E%E7%89%87), [函数参数传递](ht...