---
title: 探究Rust中有趣的设计
url: https://www.luozhiyun.com/archives/889
source: luozhiyun`s Blog
date: 2025-12-20
fetch_date: 2025-12-21T03:27:51.074043
---

# 探究Rust中有趣的设计

Search for:

Search

[Skip to content](#content)

[luozhiyun`s Blog](https://www.luozhiyun.com/)

我的技术分享

Menu

* [首页](http://www.luozhiyun.com/)
* [Go系列](https://www.luozhiyun.com/archives/category/%E5%90%8E%E7%AB%AF/go)
* [kubernetes源码系列](https://www.luozhiyun.com/archives/tag/%E6%B7%B1%E5%85%A5k8s)
* [关于我](https://www.luozhiyun.com/%E5%85%B3%E4%BA%8E)
* [RSS订阅](https://www.luozhiyun.com/feed)
* [Github](https://github.com/luozhiyun993)

# 探究Rust中有趣的设计

Posted on 2025年12月20日 by [luozhiyun](https://www.luozhiyun.com/archives/author/luoluo1993)

这篇文章主要是看一下Rust有哪些比较有意思的设计，相比其他语言之下为什么要这么设计。

## 变量的可变性

Rust 的变量在默认情况下是**不可变的**，但是可以通过 `mut` 关键字让变量变为**可变的**，让设计更灵活。也就是说，如果我们这么写，编译会报错：

```
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

报错：

```
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
```

这其实和 C++ 和 Go（以及 Java、Python 等绝大多数主流语言）的设计哲学完全相反。C++ 和 Go 默认就是可变的，除非加上 const 表示是个常量。

Rust 这样做主要是为了让代码变得清晰点，降低心智负担 。一个变量往往被多处代码所使用，其中一部分代码假定该变量的值永远不会改变，而另外一部分代码却无情的改变了这个值，在实际开发过程中，这个错误是很难被发现的，特别是在多线程编程中。再来就是编译器优化，如果编译器知道一个变量绝不会变，它可以更激进地进行常量折叠、寄存器分配等优化。

在 Rust 中，可变性很简单，只要在变量名前加一个 `mut` 即可：

```
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

但是 Rust 提供了 Shadowing 的功能：

```
fn main() {
    let x = 5;
    // x = x + 1; // 报错，不能修改

    let x = x + 1; // 合法！这是一个全新的 x，它遮蔽了旧的 x

    let x = "Hello"; // 甚至可以改变类型！
    println!("{}", x);
}
```

这点很有意思，在很多语言是不可以这么重复声明变量的。我觉得还是和不可变性有关，既然都不可变了，重复声明也是安全的，并且复用同一个变量名，而不需要想出 `x_str`, `x_int`, `x_final` 这种名字，相对来说代码会简洁一些。

## 所有权 (Ownership)

现在内存管理一般分为两类：

1. 手动管理派 (C / C++)，申请 (`malloc`/`new`)，需要手动负责释放 (`free`/`delete`)，但是这是很痛苦的，有时候忘记释放就会内存泄露，或者释放两次就会导致崩溃或为定义的行为；
2. 垃圾回收派 (Java / Go / Python)，有一个 Runtime 里的 GC (Garbage Collector) 盯着内存，不再用的就自动回收。自动回收也有缺点，需要STW，例如在游戏后端或高频交易中，几毫秒的 GC 卡顿可能就是灾难；

而 Rust 使用所有权 (Ownership)来控制。编译器在**编译阶段**通过一套严格的规则，自动在合适的地方插入 `free` 代码。没有运行时 GC，也不依赖手动管理。这个编译器定义的所有权规则有以下几条：

1. Rust 中每一个值都被一个变量所拥有，该变量被称为值的所有者
2. 一个值同时只能被一个变量所拥有，或者说一个值只能拥有一个所有者
3. 当所有者（变量）离开作用域范围时，这个值将被丢弃(drop)

比如这个例子：

```
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // 赋值操作

    // println!("{}, world!", s1); // ??? 这里能打印 s1 吗？
}
```

在 C++ 中，如果 s1 申请的是一个堆上的对象，如果是浅拷贝 (Shallow Copy)，`s1` 和 `s2` 指向堆上的同一块内存。如果函数结束，析构函数执行两次，导致 **Double Free** 错误。

在 Rust 中，由于所有权的存在，这一行 `let s2 = s1;`代码执行后，`s1` 会当场死亡！发生 **所有权转移 (Move)**。Rust 认为：堆上的那块 "hello" 内存，现在归 `s2` 管了。所以如果你后面再用 `s1`，**编译直接报错**。

报错：

```
error[E0382]: borrow of moved value: `s1`
 --> src/main.rs:5:28
  |
2 |     let s1 = String::from("hello");
  |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
3 |     let s2 = s1;
  |              -- value moved here
4 |
5 |     println!("{}, world!", s1);
  |                            ^^ value borrowed here after move
```

如果你确实需要两个独立的字符串数据（深拷贝），你需要显式调用 `.clone()`。

```
let s1 = String::from("hello");
let s2 = s1.clone(); // 在堆上重新开辟内存，复制数据

println!("s1 = {}, s2 = {}", s1, s2); // s1 依然活着
```

除此之外，要注意栈上的数据 ，对于基本类型，基本类型（存储在栈上），Rust 会自动拷贝，其他的非基本类型会存储在堆上，不能自动拷贝。

```
let x = 5;
let y = x;
```

代码首先将 `5` 绑定到变量 `x`，接着**拷贝** `x` 的值赋给 `y`，最终 `x` 和 `y` 都等于 `5`，因为整数是 Rust 基本数据类型，是固定大小的简单值，因此这两个值都是通过**自动拷贝**的方式来赋值的，都被存在栈中，完全无需在堆上分配内存。

## 借用（Borrowing）

借用（Borrowing），就是允许你在**不获取所有权 (Ownership)** 的情况下访问数据。简单来说，**借用就是创建数据的引用 (Reference)**。

借用有两种方式，不可变借用 ： `&T`，可变借用：`&mut T`。**在任意给定的作用域中，你只能满足以下两个条件之一：**

1. 拥有 **任意数量** 的不可变引用 (`&T`)。
2. 拥有 **唯一一个** 可变引用 (`&mut T`)。

即：要么多读，要么独写，绝不能同时存在，这个规则非常像 **读写锁（Read-Write Lock）**。

比如下面就是合法的借用 (多读)：

```
fn main() {
    let s = String::from("hello"); // s 拥有所有权

    let r1 = &s; // 不可变借用 1
    let r2 = &s; // 不可变借用 2

    // 可以同时存在多个读者
    println!("{}, {}", r1, r2);
} // 借用结束
```

合法的借用 (独写)：

```
fn main() {
    let mut s = String::from("hello");

    let r1 = &mut s; // 可变借用
    r1.push_str(", world"); // 修改数据

    println!("{}", r1);
}
```

非法的借用 (读写冲突)：

```
fn main() {
    let mut s = String::from("hello");

    let r1 = &s;      // r1 借用以此只读
    let r2 = &mut s;  // 错误！不能在有不可变引用的同时创建可变引用
                      // 因为 r2 可能会改变 s，导致 r1 看到的数据失效或不一致

    println!("{}, {}", r1, r2);
}
```

需要注意的是，现在的 Rust 编译器非常聪明，它的“作用域”不再仅仅是花括号 `{}`，而是看**引用的最后一次使用位置**，这叫做 **Non-Lexical Lifetimes (NLL)**。

```
fn main() {
    let mut s = String::from("hello");

    let r1 = &s;
    let r2 = &s;
    println!("{} and {}", r1, r2);
    // --- r1 和 r2 的作用在这里就结束了！因为后面没再用过它们 ---

    let r3 = &mut s; // 现在可以了！
    // 因为上面的不可变引用已经不再使用了，Rust 判定冲突解除。
    println!("{}", r3);
}
```

Rust 这么做其实也是为了安全，我们看看 C++ 中常见的坑：

```
// C++ 伪代码
vector<int> v = {1, 2, 3};
int& element = v[0]; // 获取第一个元素的引用

v.push_back(4);
// 危险！如果 push_back 导致 vector 扩容（重新分配内存），
// 原来的内存被释放，element 现在指向的是垃圾内存。
// 再次访问 element 会导致 Crash。
```

**在 Rust 中：**

* `element` 是一个不可变借用 (`&T`)。
* `v.push_back` 需要获取 `v` 的可变借用 (`&mut T`)。
* 规则冲突：已经有 `&` 了，不能再借出 `&mut`。
* **结果：编译直接报错，阻止隐患。**

## 字符串

### **`&str`** 和 **`String`**

在其他很多语言用 "hello" 这种方式创建的一般就叫字符串，但在 rust 里面不一样，它实际上是申明了一个**只读**的字符串字面量 `&str`，这意味着它是不可变的，数据直接硬编码在编译后的**可执行文件 (Binary)** 中（静态存储区），有点像 const。

```
let name = "Rust"; // 类型是 &str
println!("Hello, {}", name);
// name.push_str(" World"); // 报错！&str 不能修改
```

在 rust 里面只有使用 `String::from(...)` 声明的字符串才是我们常规意义上理解的字符串，比如可以对它进行**修改**、**拼接**和**传递**。

1. 修改字符串 (必须加 `mut`)

   ```
   fn main() {
      // 注意：如果要修改，必须加 mut 关键字
      let mut s = String::from("Hello");

      // 1. 追加字符串切片 push_str()
      s.push_str(", world");

      // 2. 追加单个字符 push()
      s.push('!');

      println!("{}", s); // 输出: Hello, world!
   }
   ```
2. 字符串拼接 (连接两个字符串)，有两种主要方式：使用 `+` 运算符或 `format!` 宏。

   ```
   fn main() {
      let s1 = String::from("Tick");
      let s2 = String::from("Tock");

      // 注意细节：
      // s1 必须交出所有权 (被移动了)，后面不能再用了
      // s2 必须传引用 (&s2)
      let s3 = s1 + " " + &s2
      // 类似 C 语言的 sprintf，生成一个新的 String
      let s4 = format!("{} - {}", s1, s2);
   }
   ```
3. `String` 可以自动假装成 `&str`

   ```
   fn main() {
      let s = String::from("Hello World");

      // 场景 1: 函数需要 String (拿走所有权)
      take_ownership(s);
      // println!("{}", s); // 报错，s 已经被拿走了

      // --- 重新创建一个 s ---
      let s = String::from("Hello Again");

      // 场景 2: 函数需要 &str (只读借用) -> 【这是最常用的】
      // 虽然 s 是 String，但 &s 可以被当做 &str 用
      borrow_it(&s);
      println!("s 还在: {}", s); // s 还在
   }

   fn take_ownership(input: String) {
      println!("我拿到了所有权: {}", input);
   } // input 在这里被释放

   fn borrow_it(input: &str) {
      println!("我只是借看一下: {}", input);
   }
   ```
4. 转换回切片 (Slicing)

   ```
   let s = String::from("Hello World");

   let hello = &s[0..5]; // 提取前5个字节
   let world = &s[6..];  // 从第6个字节取到最后
   ```

需要注意的是，Rust 的字符串在底层是 UTF-8 编码的字节数组，不支持直接通过数组下标索引（Index）访问字符。比如这样是会报错的：

```
let s1 = String::from("hello");
let h = s1[0];
```

Rust 的 `String` 本质上是一个 `Vec<u8>`（字节向量）。**对于纯英文：** `"hello"`，每个字母占 1 个字节。`s[0]` 确实是 `'h'`。**对于中文/特殊符号：** `"你好"`。在 UTF-8 中，’你’ 占用 3 个字节。

为了强迫开发者意识到 **“字符 ≠ 字节”** 这一事实，Rust 干脆在编译阶段就禁止了 `String[index]` 这种写法。

所以，为了获取第 N 个字符 (最常用，安全)，需要使用 `.chars()` 迭代器：

```
fn main() {
    let s1 = String::from("hello");

    // .chars() 把字符串解析为 Unicode 字符
    // .nth(0) 取出第 0 个元素
    // 结果是 Option<char>，因为字符串可能是空的
    match s1.chars().nth(0) {
        Some(c) => println!("第一个字符是: {}", c),
        None => println!("字符串是空的"),
    }
}
```

### 为什么要有这两个？

* **`String`** 是为了当你需要在运行时**动态生成**、**修改**或者**持有**字符串数据时使用的（比如从网络读取数据，拼接 SQL）。
* **`&str`** 是为了**高性能传递**。当你只需要“看”一下字符串，而不需要拥有它时，用 `&str`。因为它只是传两个整数（指针+长度），不需要拷贝堆上的数据，速度极快。

## 枚举

Rust 的枚举和其他语言最大的不同应该就是能挂载数据。每一个枚举成员，都可以关联不同类型、不同数量的数据。

```
enum Message {
    Quit,                       // 没有关联数据
    Move { x: i32, y: i32 },    // 像 Struct 一样包含命名字段
    Chat(String),               // 包含一个...