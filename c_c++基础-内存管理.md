---
title: C/C++基础合集：内存管理
layout: post
date: 2020-06-16 14:56:00 +0800
categories: 秋招
tags:
	- c/c++
	- 面经
	- 秋招
---

内存管理是程序开发时需要考虑的重要的环节，包括数据的存放，内存的开辟与释放。

[TOC]

---

# 内存模型

32bit操作系统：

<img src="https://gitee.com/zyuegege/images/raw/master/imgs/linuxFlexibleAddressSpaceLayout.png" alt="Flexible Process Address Space Layout In Linux" style="zoom:80%;" />

64bit操作系统：

![x86_process_address_space.png](https://gitee.com/zyuegege/images/raw/master/imgs/x86_process_address_space.png)

## 各分区的意义

-   **内核空间**： 在32位系统中，Linux会留1G空间给内核，用户进程是无法访问的，用来存放进程相关数据和内存数据，内核代码等； 在64位系统里面，Linux会采用最低48位来表示虚拟内存，这可通过 /proc/cpuinfo 来查看address sizes
-   **stack栈区**：专门用来实现函数调用-栈结构的内存块。相对空间下（可以设置大小，Linux 一般默认是8M，可通过 ulimit –s 查看），系统自动管理，从高地址往低地址，向下生长。
-   **内存映射区**： 包括文件映射和匿名内存映射， 应用程序的所依赖的动态库，会在程序执行时候，加载到内存这个区域，一般包括数据（data）和代码（text）;通过mmap系统调用，可以把特定的文件映射到内存中，然后在相应的内存区域中操作字节来访问文件内容，实现更高效的IO操作；匿名映射，在glibc中malloc分配大内存的时候会用到匿名映射。这里所谓的“大”表示是超过了`MMAP_THRESHOLD` 设置的字节数，它的缺省值是 128 kB，可以通过 [`mallopt()`](http://www.kernel.org/doc/man-pages/online/pages/man3/undocumented.3.html) 去调整这个设置值。还可以用于进程间通信IPC（共享内存）。
-   **heap堆区**：主要用于用户动态内存分配，空间大，使用灵活，但需要用户自己管理，通过brk系统调用控制堆的生长，向高地址生长。
-   **BBS段和DATA段**：用于存放程序全局数据和静态数据，一般未初始化的放在BSS段（统一初始化为0，不占程序文件的空间），初始化的放在data段，只读数据放在rodata段（常量存储区）。
-   **text段**： 主要存放程序二进制代码。

1.  为了防止内存被攻击，比如栈溢出攻击和堆溢出攻击等，Linux在特定段之间使用随机偏移，使段的起始地址是随机值。

    Linux 系统上的 ASLR 等级可以通过文件 /proc/sys/kernel/randomize_va_space 来进行设置，它支持以下取值：

    -   0 – 关闭的随机化。一切都是静止的。
    -   1 – 保守的随机化。共享库、栈、mmap()、VDSO（下面有说明）以及堆将被随机化。
    -   2 – 完全的随机化。除了上面列举的要素外，通过 brk() 分配得到的内存空间也将被随机化。

2.  每个段都有特定的安全控制（权限）：

    |          |                |                                                              |
    | -------- | :------------: | ------------------------------------------------------------ |
    | vm_flags | 第三列，如r-xp | 此段虚拟地址空间的属性。每种属性用一个字段表示，r表示可读，w表示可写，x表示可执行，p和s共用一个字段，互斥关系，p表示私有段，s表示共享段，如果没有相应权限，则用’-’代替 |

3.  Linux虚拟内存是按页分配，每页大小为4KB或者2M（大页内存），默认是4K

例子-通过`pmap` 查看程序内存布局(综合`proc/x/maps`与`proc/x/smaps`数据）

```bash
pmap -X 8117
```

## 问题

1.  栈为什么要由高地址向低地址扩展，堆为什么由低地址向高地址扩展？

    历史原因：在没有MMU的时代，为了最大的利用内存空间，堆和栈被设计为从两端相向生长。那么哪一个向上，哪一个向下呢？人们对数据访问是习惯于向上的，比如你在堆中new一个数组，是习惯于把低元素放到低地址，把高位放到高地址，所以堆 向上生长比较符合习惯, 而栈则对方向不敏感，一般对栈的操作只有PUSH和pop，无所谓向上向下，所以就把堆放在了低端，把栈放在了高端. 但现在已经习惯这样了。这个和处理器设计有关系，目前大多数主流处理器都是这样设计，但ARM 同时支持这两种增长方式。

2.  如何查看进程虚拟地址空间的使用情况？

3.  对比堆和栈优缺点？

---

# C++堆内存空间模型

## C++ 程序动态申请内存new/delete

1.  new/delete 操作符,C++内置操作符：

    -   new操作符做两件事，分配内存+调用构造函数初始化。你不能改变它的行为；
    -   delete操作符同样做两件事，调用析构函数+释放内存。你不能改变它的行为；

2.  operator new/delete 函数：

    -   operator new 是用来专门分配内存的函数，为new操作符调用，你能增加额外的参数重载函数operator new（有限制）:
        -   第一个参数类型必须是size_t；
        -   函数必须返回void*；
    -   operator new 底层一般调用malloc函数（gcc+glibc）分配内存；
    -   operator new 分配失败会抛异常（默认），通过传递参数也可以不抛异常，返回空指针；
    -   operator delete  是用来专门分配内存的函数，为delete操作符调用，你能增加额外的参数重载函数operator delete（有限制）:
        -   第一个参数类型必须是void*；
        -    函数必须返回void；
    -   operator delete底层一般调用free函数（gcc+glibc）释放内存；
    -   operator delete分配失败会抛异常（默认），通过传递参数也可以不抛异常，返回空指针；

3.  placement new/delete 函数

    -   placement new 其实就是new的一种重载，placement new是一种特殊的operator new，作用于一块已分配但未处理或未初始化的raw内存，就是用一块已经分配好的内存上重建对象（调用构造函数）；
    -   它是C++库标准的一部分；
    -   placement delete 什么都不做；

4.   数组分配 new[]/delete[] 表达式

    -   对应会调用operator new[]/delete[]函数;
    -   按对象的个数，分别调用构造函数和析构函数；

5.  class-specific allocation functions

    ![](https://gitee.com/zyuegege/images/raw/master/imgs/2020-06-16%2016-31-04%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

    定制对象特殊new/delete函数；实现一般是使用全局：

    ```c++
    ::operator new
    ::operator delete
    ```

关键点：

-   你想在堆上建立一个对象，应该用new操作符。它既分配内存又为对象调用构造函数。

-   如果你仅仅想分配内存，就应该调用operator new函数；它不会调用构造函数。

-   如果你想定制自己的在堆对象被建立时的内存分配过程，你应该写你自己的operator new函数，然后使用new操作符，new操作符会调用你定制的operator new。

-   如果你想在一块已经获得指针的内存里建立一个对象，应该用placement new。

-   C++可以为分配失败设置自己的异常处理函数：

     If [set_new_handler](http://www.cplusplus.com/set_new_handler) has been used to define a [new_handler ](http://www.cplusplus.com/new_handler)function, this *new-handler* function is called by the default definitions of the allocating versions (*(1)* and *(2)*) if they fail to allocate the requested storage.

-   如果在构造函数时候抛出异常，new表达式后面会调用对应operator delete函数释放内存：

    The other signatures (*(2)* and *(3)*) are **never** called by a *delete-expression* (the `delete` operator always calls the ordinary version of this function, and exactly once for each of its arguments). These other signatures are only called automatically by a *new-expression* when their object construction fails (e.g., if the constructor of an object throws while being constructed by a *new-expression* withnothrow, the matchingoperator deletefunction accepting anothrowargument is called).

## 问题

1.  malloc和free是怎么实现的？
2.  malloc 分配多大的内存，就占用多大的物理内存空间吗？
3.  free 的内存真的释放了吗（还给 OS ） ?
4.  既然堆内内存不能直接释放，为什么不全部使用 mmap 来分配？
5.  如何查看堆内内存的碎片情况？
6.  除了 glibc 的 malloc/free ，还有其他第三方实现吗？

---

# 智能指针

-   C++智能指针出现是为了解决由于支持动态内存分配而导致的一些C++内存问题，比如内存泄漏， 对象生命周期的管理，悬挂指针（dangling pointer）/空指针等问题；
-   C++智能指针通过RAII设计模式去管理对象生命周期（动态内存管理），提供带少量异常类似普通指针的操作接口，在对象构造的时候分配内存，在对象作用域之外释放内存，帮助程序员管理动态内存；
-   老的智能指针`auto_ptr`由于设计语义不好而导致很多不合理问题： 不支持复制（拷贝构造函数）和赋值（operator =），但复制或赋值的时候不会提示出错。因为不能被复制，所以不能被放入容器中。而被C++11弃用（deprecated）。

## shared_ptr

`shared_ptr` 是引用计数型（reference counting）智能指针, `shared_ptr`包含两个成员，一个是指向真正数据的指针，另一个是引用计数`ref_count`模块指针,对比GCC实现，大致原理如下，

![img](https://gitee.com/zyuegege/images/raw/master/imgs/1389958-20180702223322013-1340666192.png)

共享对象（数据）（赋值拷贝），引用计数加1，指针消亡，引用计数减1，当引用计数为0，自动析构所指的对象，引用计数是线程安全的（原子操作）。

`enable_shared_from_this`模板类,用来返回`this`指针的`shared_ptr`版本

shared_ptr关键点：

1.  用shared_ptr就不要new，保证内存管理的一致性;

2.  使用weak_ptr来打破循环引用;

3.  用make_shared来生成shared_ptr：

    -   提高效率，内存分配一次搞定（对象数据和ref_count控制模块）；
    -    防止异常导致内存泄漏，参考https://herbsutter.com/gotw/_102/;
    -   由于一次性分配内存，对象数据和ref_count控制模块生命周期被绑定在一起，需要等到所有的weak引用为0时才能最终释放内存（delete），当use_count为0时只会调用析构函数；//from http://en.cppreference.com/w/cpp/memory/shared_ptr/make_shared，所以对象内存的占用时间比较长；

4.  用enable_shared_from_this来使一个类能获取自身的shared_ptr;

5.  不能在对象的构造函数中使用shared_from_this()函数，为什么?

    因为对象还没有构造完毕，share_ptr还没有初始化构造完全。 构造顺序：先需要调用enable_shared_from_this类的构造函数，接着调用对象的构造函数，最后需要调用shared_ptr类的构造函数初始化enable_shared_from_this的成员变量weak_this_。然后才能使用shared_from_this()函数。

6.  大量的shared_ptr会导致程序性能下降（相对其他指针）。

## unique_ptr

独占指针，不共享,不能赋值拷贝；

`unique`_ptr关键点：

1.  如果对象不需要共享，一般最好都用unique_ptr，性能好，更安全；

2.  可以通过move语义传递对象的生命周期控制权；

3.  函数可以返回unique_ptr对象,为什么？

    当函数返回一个对象时，理论上会产生临时变量，那必然是会导致新对象的构造和旧对象的析构，这对效率是有影响的。C++编译针对这种情况允许进行优化，哪怕是构造函数有副作用，这叫做返回值优化（RVO),返回有名字的对象叫做具名返回值优化(NRVO)，就那RVO来说吧，本来是在返回时要生成临时对象的，现在构造返回对象时直接在接受返回对象的空间中构造了。假设不进行返回值优化，那么上面返回unique_ptr会不会有问题呢？也不会。因为标准允许编译器这么做：

    -   如果支持move构造，那么调用move构造。
    -   如果不支持move，那就调用copy构造。
    -   如果不支持copy，那就报错吧。

    显然的，unique_ptr是支持move构造的，unique_ptr对象可以被函数返回。

## weak_ptr

引用对象，不增加引用计数，对象生命周期，无法干预；配合shared_ptr解决shared_ptr循环引用问题；可以影响到对象内存最终释放的时间

## 问题

1.  C++的赋值和Java的有什么区别？

    C++的赋值可以是对象拷贝也可以对象引用，java的赋值是对象引用；

2.  smart_ptr有哪些坑可以仍然导致内存泄漏？

    -    shared_ptr初始化构造函数指针，一般是可以动态管理的内存地址，如果不是就可能导致内存泄漏；
    -    shared_ptr要求内部new和delete实现必须是成对，一致性，如果不是就可能导致内存泄漏；
    -   shared_ptr对象和其他大多数STL容器一样，本身不是线程安全的，需要用户去保证；

3.  unique_ptr有哪些限制？

    -   只能移动赋值转移数据，不能拷贝；
    -   不支持类型转换（cast）；
    
4.  智能指针是异常安全的吗？

    所谓异常安全是指,当异常抛出时，带有异常安全的函数会:

    -   不泄露任何资源
    -   不允许数据被破坏

    智能指针就是采用RAII技术，即以对象管理资源来防止资源泄漏。

    Several functions in these smart pointer classes are specified as having "no effect" or "no effect except such-and-such" if an exception is thrown. This means that when an exception is thrown by an object of one of these classes, the entire program state remains the same as it was prior to the function call which resulted in the exception being thrown. This amounts to a guarantee that there are no detectable side effects. Other functions never throw exceptions. The only exception ever thrown by functions which do throw (assuming **T** meets the [common requirements](https://www.boost.org/doc/libs/1_61_0/libs/smart_ptr/smart_ptr.htm#common_requirements)) is **std::bad_alloc**, and that is thrown only by functions which are explicitly documented as possibly throwing **std::bad_alloc**.

5.  智能指针是线程安全的吗？

    智能指针对象的引用计数模块是线程安全的，因为 shared_ptr 有两个数据成员，读写操作不能原子化，所以对象本身不是线程安全的，需要用户去保证线程安全。

    `shared_ptr` objects offer the same level of thread safety as built-in types. A `shared_ptr` instance can be "read" (accessed using only const operations) simultaneously by multiple threads. Different `shared_ptr` instances can be "written to" (accessed using mutable operations such as `operator=` or `reset`) simultaneously by multiple threads (even when these instances are copies, and share the same reference count underneath.)

    Any other simultaneous accesses result in undefined behavior.

6.  C++可以通过哪些技术来支持“垃圾回收”？

     smart_ptr，RAII， move语义等；

7.   RAII是指什么？

      RAII是指Resource Acquisition Is Initialization的设计模式，

     RAII要求，资源的有效期与持有资源的对象的生命期严格绑定，即由对象的构造函数完成资源的分配(获取)，同时由析构函数完成资源的释放。在这种要求下，只要对象能正确地析构，就不会出现资源泄露问题。
    当一个函数需要通过多个局部变量来管理资源时，RAII就显得非常好用。因为只有被构造成功(构造函数没有抛出异常)的对象才会在返回时调用析构函数[4]，同时析构函数的调用顺序恰好是它们构造顺序的反序[5]，这样既可以保证多个资源(对象)的正确释放，又能满足多个资源之间的依赖关系。

    由于RAII可以极大地简化资源管理，并有效地保证程序的正确和代码的简洁，所以通常会强烈建议在C++中使用它。

---

# STL内存模型

STL（C++标准模板库）引入的一个Allocator概念。整个STL所有组件的内存均从allocator分配。也就是说，STL并不推荐使用 new/delete 进行内存管理，而是推荐使用allocator。SGI STL allocator设计：

![空间配置器](https://gitee.com/zyuegege/images/raw/master/imgs/006tNc79gw1fbkfode4zyj30qs0agq3y.jpg)

对象的构造和析构采用placement new函数

![构造和析构](https://gitee.com/zyuegege/images/raw/master/imgs/006tNc79gw1fbkfo9n8psj30ql08z3zm.jpg)

![内存配置](https://gitee.com/zyuegege/images/raw/master/imgs/006tNc79gw1fbkfoeivm3j30q3078t9m.jpg)

![二级配置器1](https://gitee.com/zyuegege/images/raw/master/imgs/006tNc79gw1fbkfocm8q6j30ri0dl40t.jpg)

## 问题

1.  vector内存设计和array的区别和适用的场景？
2.  遍历map与遍历vector哪个更快，为什么？
3.  STL的map和unordered_map内存设计各有什么不同？

以上引用[C++内存模型](https://www.cnblogs.com/alexcool/p/9241548.html)

