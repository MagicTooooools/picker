---
title: Java Strings的秘密
url: https://www.ttalk.im/s/XMbTbz
source: The Talk
date: 2025-12-25
fetch_date: 2025-12-26T03:26:44.635666
---

# Java Strings的秘密

# Java Strings的秘密

JVM

这篇文章深入讲解了 Java 字符串的内部实现机制，主要涵盖以下几个核心主题：

**1. String 的不可变性**

String 一旦创建就无法修改，这带来了线程安全、安全性、可缓存（字符串池）以及哈希码缓存等优势。

**2. 字符串存储与编码的演进**

JDK 9 之前，所有字符都使用 UTF-16 编码存储，每个字符占用 2 字节。JDK 9 引入了 **Compact Strings**（紧凑字符串，JEP 254），JVM 会自动选择编码方式：对于纯 ASCII 字符串使用 LATIN1（每字符 1 字节），其他情况使用 UTF16。这个优化对应用代码透明，可将内存占用降低多达 50%。

JDK 18 起（JEP 400），默认字符集统一为 UTF-8，解决了此前不同操作系统默认编码不一致的问题。

**3. 字符串池与 intern 机制**

字符串字面量会自动进入字符串池，`intern()` 方法可手动将字符串加入池中。虽然能节省内存并加快比较速度（用 `==` 代替 `equals()`），但也有缺点：池本身消耗内存、intern 操作有同步开销、可能导致内存泄漏或被恶意利用。现代 JVM 提供了 **String Deduplication** 功能（需启用 `-XX:+UseStringDeduplication`），可在 GC 时自动去除重复字符串，是更安全的替代方案。

**4. 字符串拼接优化**

编译器会对字面量拼接进行编译时优化。JDK 9 之前，运行时拼接会被编译成 StringBuilder 操作；JDK 9 之后改用 `invokedynamic`，让 JVM 在运行时选择最优策略。

文章通过 JMH 基准测试展示了在循环中拼接字符串的性能差异：使用 `+` 操作符在 10000 次迭代时需要约 37ms，而使用 StringBuilder 仅需约 109μs，预分配容量的 StringBuilder 更是只需约 91μs——差距可达 **300 倍以上**。

最后，文章对比了 StringBuffer（线程安全但较慢）和 StringBuilder（非线程安全但更快），建议在单线程场景优先使用 StringBuilder。

扫描二维码分享此链接

![QR Code](data:image/png;base64...)

[🔗 访问目标链接](https://tanis.codes/posts/java-strings-internals/)

Short code: XMbTbz • Powered by Owl