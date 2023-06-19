## Java-基础

- 面向对象的三大特性：封装、继承、多态（方法重载、方法重写）

- 解释执行：将编译好的字节码逐条翻译为机器码执行。
  编译执行：以方法为单位，将字节码一次性翻译为机器码后执行。

  目前主流的JVM 都是混合模式，即解释运行 和编译运行配合使用。

  在HotSpot虚拟机中，提供了两种编译模式：解释执行 和 即时编译（JIT，Just-In-Time）。前者的优势在于不用等待，后者则在实际运行当中效率更高。

  即时编译存在的意义在于它是提高程序性能的重要手段之一。根据“二八定律”（即：百分之二十的代码占据百分之八十的系统资源），对于大部分不常用的代码，我们无需耗时间将之编译为机器码，而是采用解释执行的方式，用到就去逐条解释运行；
  对于一些仅占据小部分的热点代码（可认为是反复执行的重要代码），则可将之翻译为符合机器的机器码高效执行，提高程序的效率，此为运行时的即时编译。

  为了满足不同的场景，HotSpot虚拟机内置了多个即时编译器：C1,C2与Graal。

  在Java 9引入了一种新的编译模式AOT（Ahead of Time Compilation），它是直接将字节码编译成机器码，这样避免了JIT预热等各方面的开销。JDK支持分层编译和AOT协作使用，但是，AOT编译器的编译质量是不及JIT编译器的

- 在 Java 8 中，Integer 缓存池的大小默认为 -128 ~127。

- Java 8 开始，接口中允许有非抽象的default方法，接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。从 Java 9 开始，允许将方法定义为 private。

  接口的字段默认都是 static 和 final 的。

  相应的，抽象类中，可以有非public的成员，非抽象的方法。

- Java中的类是单继承多实现，而接口是支持多继承的。

- Comparable接口的compareTo()和Comparator接口中的compare()，前者是内比较器，后者是外比较器

- Throwable Error Exception（受检异常，非受检异常）

- 在 Java 中的反射机制是指在运行状态中，对于任意一个类都能够知道这个类所有的属性和方法;并且对于任意一个对象，都能够调用它的任意一个方法;这种动态获取信息以及动态调用对象方法的功能称为 Java 语言的反射机制。

- RPC(Remote Procedure Call 远程过程调用)，核心流程：client stub、server stub

- Java RMI(Java Remote Method Invocation 远程方法调用) 是 Java 编程语言里，一种用于实现远程过程调用的应用程序编程接口

- Java中只有值传递，这个值可以是基本变量的值，也可以是引用类的地址。

- Java 8 对时间日期的API做了很大的优化，引入了LocalDate、LocalTime、LocalDateTime、 Clock、Instant 等类

- Java 中读取 long 类型变量不是原子的，需要分成两步，如果一个线程正在修改该 long 变量的值，另一个线程可能只能看到该值的一半(前 32 位)。但是对一个 volatile 型的 long 或 double 变量的读写是原子。

- 编译期常量（public static final）在编译时会被替换掉。

- 多线程原子操作AtomicXXX原子类、XXXAdder加法器、XXXAccumulator累加器。XXXAdder，通过尝试使用分段CAS以及自动分段迁移的方式来大幅度提升多线程高并发执行CAS操作的性能！

### IO

- 阻塞IO模型、非阻塞IO模型、多路复用IO模型（有一个线程不断去轮询多个 socket 的状态，只有当 socket 真正有读写事件时，才真正调用实际的 IO 读写操作）、信号驱动IO模型、异步IO模型

  在信号驱动 IO 模型中，当用户线程接收到信号表示数据 已经就绪，然后需要用户线程调用 IO 函数进行实际的读写操作;而在异步 IO 模型中，收到信号表示 IO 操作已经完成，不需要再在用户线程中调用 IO 函数进行实际的读写操作。

- NIO 主要有三大核心部分:Channel(通道)，Buffer(缓冲区), Selector(选择区)。传统 IO 基于字节流和字符流进行操作，而 NIO 基于 Channel 和
  Buffer进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector用于监听多个通道的事件。因此，单个线程可以监听多个数据通道。

  NIO 实现了 IO 多路复用中的 Reactor 模型，一个线程 Thread 使用一个选择器 Selector 通过轮询的方式去监听多个通道 Channel 上的事件，从而让一个线程就可以处理多个事件。

- NIO 和传统 IO 之间第一个最大的区别是，IO 是面向流的，NIO 是面向缓冲区的。

- 字节流与字符流转换：InputStreamReader、OutputStreamReader

- 基于IO与基于NIO的文件读写

- 内存映射文件

  内存映射文件 I/O 是一种读和写文件数据的方法，它可以比常规的基于流或者基于通道的 I/O 快得多。

  向内存映射文件写入可能是危险的，只是改变数组的单个元素这样的简单操作，就可能会直接修改磁盘上的文件。修改数据与将数据保存到磁盘是没有分开的。

  下面代码行将文件的前 1024 个字节映射到内存中，map() 方法返回一个 MappedByteBuffer，它是 ByteBuffer 的子类。因此，可以像使用其他任何 ByteBuffer 一样使用新映射的缓冲区，操作系统会在需要时负责执行映射。

  ```java
  MappedByteBuffer mbb = fileChannel.map(FileChannel.MapMode.READ_WRITE, 0, 1024);
  ```
- mmap 映射的过程可以理解为一个懒加载， 只有 get() 时才会触发缺页中断
- 预读大小是有操作系统算法决定的，可以默认当作 4kb，即如果希望懒加载变成实时加载，需要按照 step=4kb 进行一次遍历

### Tools
- Maven可以进行debug，具体是采用mvndebug代替mvn命令，之后即可进行remote debug

