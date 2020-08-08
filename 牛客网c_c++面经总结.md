---
title: 牛客网c/c++面经总结
layout: post
date:   2020-06-07 10:35:09 +0800
categories: 秋招
tags:
	- c/c++
	- 面经
	- 秋招
---

vivo提秋招前批的开始，拉开了2021秋招的序幕，为了准备秋招，整理了牛客网上面关于[c/c++面经合集](https://www.nowcoder.com/discuss/experience?tagId=640&order=3&companyId=0)从2020-04-27到2020-06-09的相关面试问题的整理，后面将持续更新。

---

[TOC]

---

# c/c++ 基础

1. **map的实现？与哈希表区别？**

2. **vector实现？**

3. **C++面向对象的三种特性？**

4. **C++继承和组合？你在实际项目中是怎么使用的？什么情况下使用继承？什么情况下使用组合？**

5. **C++构造函数可以是虚函数吗？为什么？**

6. **C++析构函数可以是虚函数吗？为什么要将析构函数设置为虚函数？**

7. **C++如何实现多态？虚表指针是什么时候被初始化的？实例化一个对象需要那几个阶段？**

8. **C++偏特化？**

9. **重载、重写、覆盖？**

10. **static关键字的作用？（要全面）怎么实现的？**

    -   在函数体外（全局作用域），限制函数或者对象的使用范围为本文件；
    -   在函数体内的静态变量，声明周期为整程序运行周期；
    -   类的静态方法与静态成员

    [https://www.cnblogs.com/XuChengNotes/p/10403523.html](https://www.cnblogs.com/XuChengNotes/p/10403523.html)

11. **inline和宏定义的区别？inline是如何实现的？宏定义是如何实现的？**

    [C++ inline和#define宏的区别](https://www.cnblogs.com/yanwei-wang/p/8073111.html)

12. **指针和引用的区别？怎么实现的？**

    [https://www.zhihu.com/question/37608201](https://www.zhihu.com/question/37608201)

13. **字节对齐？结构体大小计算？然后又扯到位域？**

    -   结构体第一个成员的地址和结构体的首地址相同
    -   结构体每个成员地址相对于结构体首地址的偏移量（offset）是该成员大小的整数倍，如果不是则编译器会在成员之间添加填充字节（internal adding）。
    -   结构体总的大小要是其成员中最大size的整数倍，如果不是编译器会在其末尾添加填充字节。

    [https://blog.csdn.net/weixin_42423872/article/details/81229249](https://blog.csdn.net/weixin_42423872/article/details/81229249)

14. **allocator了解吗**

     [C++ allocator类学习理解](https://www.cnblogs.com/SimonKly/p/7819122.html)、[C++中的allocator类（内存分配器）](https://blog.csdn.net/u012333003/article/details/22104939)、[C++中为什么要有allocator类？](https://www.zhihu.com/question/274802525)、[C++ allocator 自定义指南](http://www.mamicode.com/info-detail-2341387.html)、[《STL源码剖析》提炼总结：空间配置器(allocator)](https://zhuanlan.zhihu.com/p/34725232)

15. **map和unordered_map的底层实现？目前map支持map[10] = 20这种操作，其底层怎么实现的？和insert有什么区别？**

16. **map如何遍历删除所有元素？迭代器失效？**

17. **一个unique_ptr怎么赋值给另一个unique_ptr对象？（std::move）**

18. **下面代码有什么问题？引用计数如何变化？**

    ```c++
    int *sp = new int{10};
    shared_ptr<int> sp1(sp);
    shared_ptr<int> sp2(sp);
    ```

    一个地址被连个智能指针所引用，引用计数都是分别计数，所以在释放的时候会释放两次。

19. **c语言的函数调用栈底层详细说说？**  

     [C语言函数调用栈(一)](https://www.cnblogs.com/clover-toeic/p/3755401.html)、[C语言函数调用栈(二)](https://www.cnblogs.com/clover-toeic/p/3756668.html)、[c语言函数调用过程中栈的工作原理理解](https://blog.csdn.net/qq_31567335/article/details/84782202)、[C语言函数调用的底层机制](https://blog.csdn.net/maochengtao/article/details/61630370)

20. **static变量存放在哪里？什么时候分配的内存？(这里感觉面试官优点不太清楚，我就详细说了说)**

22. 写道题吧：非递归求二叉树的高度？（需要写测试用例）

23. **多态、vector，hashtable，set/map的实现**、**智能指针**、**C++11**；

24. **short类型占几个字节，int 类型占几个字节，short和int相加时会有隐式类型转换，对于补码来说，这个转换是在short的高字节位填充什么？把一个负数转换为补码形式最简单的方法应该是怎么做？**

25. **带参数的宏定义怎么写？**

25. **strlen这个函数如果要自己写应该怎么实现？写了几行简单代码（但我总觉得太简单了，自己的考虑可能不够周全，又短时间内不知道落下了什么。。。）**

    [strlen函数的三种实现方式](https://blog.csdn.net/ttyue_123/article/details/52166435)

26. **局部变量和全局变量的作用范围和生存周期，外部变量要在另一个源文件中使用要怎么做？加static之后会怎么样？**

27. **new的对象可以free吗？为什么？**

    不可以。

28. **为什么析构函数要虚函数**

29. **map底层，和avl比有什么优势，**

    map底层使用的是红黑树实现，红黑树牺牲了avl的完全平衡性减少了维持平衡和需要的旋转次数，使得在插入和删除市最多只旋转3次（插入两次）。使得红黑树在插入和删除时更快。并且查找时间也是非常快的。

30. **c++如何实现多态？多态是什么？重写是什么？**

31. **C++ 的vector的底层数据结构是什么？为什么2倍扩容？**

32. **对于模板了解什么，了解模板底层原理，你对模板的理解是什么？**

33. **虚函数什么时候用？**

34. **c++析构函数，尽可能把你理解的析构函数介绍一下？**
    析构函数是为了对对象的一些资源进行释放，析构函数在一些具体的代码中要设置成为虚析构函数，先析构子类

35. **c++的基本数据类型的长度？**

36. **对面向对象有什么理解？虚继承？接口继承和实现继承有什么区别？虚基类？**

37. 无锁编程 atomic，**STL 空间配置器源码，shared_ptr源码**

38. **C++ 中的map是什么？**与python中的map有什么不同？

39. **拷贝构造函数：深拷贝和浅拷贝**

40. **new函数失败怎么办，**

    我答，抛出bad_alloc异常，可以通过try catch捕捉。或者通过set_new_handler函数设置函数指针处理失败后的异常。  
    再问：可以不抛出异常吗？  
    答：可以，new时，添加std::nothrow  
    三问，set_new_handler函数怎么实现的？  
    具体函数实现我没看，不过我猜应该是通过设置signal或者sigaction捕捉信号函数，捕捉到异常的信号后，再用回调函数处理的。

41. **C++11的lambda是如何实现的，它跟真正的仿函数在实现上有什么区别，lambda内部可以有一个局部变量但是仿函数没有，是为什么**

    [lambda表达式与仿函数](https://blog.csdn.net/quinta_2018_01_09/article/details/93763832)、[C++ 仿函数](https://blog.csdn.net/K346K346/article/details/82818801)

42. **dynamic_cast，解决什么问题，引用的情况下不成功返回什么（我说不会，然后让我设计一下，我说抛异常）**

43. **C语言中printf中无限参数的问题，怎样实现？**

    [C语言中的可变参数（printf的实现原理）](https://blog.csdn.net/yzy1103203312/article/details/80808741)、[C语言实现printf函数，即参数可变函数原理](https://blog.csdn.net/gengyiping18/article/details/50603326)

44. **对封装的理解，抽象基类**

45. **++i， i++是否是原子操作** 

    [i++和++i是否为原子操作](https://blog.csdn.net/u010214092/article/details/50634430)、[i++是否原子操作](https://www.cnblogs.com/lxd2502/p/4648020.html)

46. **template 怎么理解“泛型”**

47. **虚函数类的对象去memoryset内存清零，有什么问题；虚表在哪里存储**

48. **析构函数可不可以是私有的；什么情况下会把它私有；**

49. **请用C写出实现atoi的程序，要求效率**

    见LeetCode:[8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

50. **闭包**

    [c++11 闭包的实现](https://www.cnblogs.com/lsgxeva/p/7788259.html)、[c++11 lambda表达式](https://www.cnblogs.com/lsgxeva/p/7788555.html)

51. **std::function可以封装哪些实体，可以封装函数对象吗？**

     [c++11 闭包的实现](https://www.cnblogs.com/lsgxeva/p/7788259.html)

52. **lambda表达式介绍一下，可以引用外部变量吗？**

    [C++ lambda表达式及其原理](https://blog.csdn.net/weixin_39640298/article/details/84996642)、[C++lambda表达式](https://blog.csdn.net/Sum7mer/article/details/89636651)

53. **std::bind和lambda表达式区别？**

    [c++11 闭包的实现](https://www.cnblogs.com/lsgxeva/p/7788259.html)、[c++11 lambda表达式](https://www.cnblogs.com/lsgxeva/p/7788555.html)

54. **为什么需要this指针**

55. **自增运算符的步骤（内存，寄存器），有两个线程分别自增100次，问最后结果的最大最小值？**

    [面试---i++在两个线程分别执行100次，最大值和最小值分别多少](https://blog.csdn.net/will130/article/details/48714343?locationNum=5)

56. 实现memcpy函数。

57. **c++使用c函数一样吗，c++编译和c编译有什么不同**

    [C/C++：函数的编译方式与调用约定以及extern “C”的使用](https://www.cnblogs.com/qinfengxiaoyue/archive/2013/02/04/2891908.html)、[C/C++ 编译与函数的深度理解](https://www.jianshu.com/p/fb4cf4d07a18)

58. gdb多线程调试

59. **公有继承、保护继承和私有继承。**

60. **switch的case里为何不能定义变量**

     [switch-case内不能定义变量？](https://www.cnblogs.com/Younger-Zhang/p/11316290.html)、[switch - case 中不能定义变量 的解决办法](https://blog.csdn.net/qq_35865125/article/details/104739997)、[请教switch内部的变量定义问题？](https://www.zhihu.com/question/23051685)

61. **如果可以确定一个类没有子类，这个类的析构函数还有必要是虚函数吗**

    没有必要，因为一旦设置成虚函数，则对象中一定会有虚函数表占用额外的存储空间，况且虚函数的调用需要从虚函数表里面查询，会额外没必要的增加调用虚函数的成本。

62. **迭代器说一下，迭代器失效了解吗？什么情况下会出现迭代器失效？**

63. **STL算法的`remove`方法和`erase`方法的区别**

    `vector`中的`remove`的作用是将等于`value`的元素放到`vector`的尾部，但并不减少`vector`的`size`

    `vector`中`erase`的作用是删除掉某个位置`position`或一段区域（begin, end)中的元素，减少其size

    `list`容器中的`remove` 成员函数，原型是v`oid remove (const value_type& val);`作用是删除`list`中值与`val`相同的节点，释放该节点的资源。而`list`容器中的`erase`成员函数，原型是`iterator erase (iterator position);`作用是删除`position`位置的节点。考虑到`list::erase`是与位置有关，故`erase`还存在API:  `iterator erase (iterator first, iterator last);`

    对于`set`来说，只有`erase` API，没有`remove` API。 `erase` 的作用是把符合要求的元素都删掉。

    -   `void erase (iterator position);`

    -   `size_type erase (const value_type& val);`

    -   `void erase (iterator first, iterator last);`

    综上所述，`erase`一般是要释放资源，真正删除元素的，而`remove`主要用在`vector`中，用于将不符合要求的元素移到容器尾部，而并不删除不符合要求的元素。

    以上引用[STL：remove和erase区别](https://www.cnblogs.com/buptlyn/p/4379212.html)

64. **SFINAE 技术**

65. **元编程，type_trait的理解**

66. **迭代器的作用**

67. **C语言中变长参数的实现原理**

68. **`std::forward`的作用**

    完美转发，利用应用的折叠原则：`T& && -> T&, T&& && -> T&&`来完成完美转发。

69. **为什么系统调用比普通函数调用消耗大？**

    切换用户栈，内核栈，保存寄存器信息，这些只是基本开销。还有页表切换，不同用户的页表是不一样的，系统调用为了服务不同用户，需要切换页表。切换页表意味着cpu缓存失效。而且内核空间跟用户空间地址空间相差很大，指令预测系统也会失效。系统调用需要在用户空间和内核空间进行切换，普通函数调用只是同一个空间调用，只是压栈这一点开销，相对于系统调用这点开销并不算什么，甚至函数式编程的压栈开销都没有。

    [https://www.zhihu.com/question/400440365](https://www.zhihu.com/question/400440365)

70. **可以直接交换两个同类型对象的内存数据吗？**

    c/c++中没有类似的命令与操作不过可以自己重载与函数等等从语义上实现交换

71. **c++ 11中union的变量可以为非POD类型吗？**

    c++11之前是不允许的，但是在c++11之后消除了这个限制，但是如果是union包含非POD类型，则需要自定义默认构造函数。

     [C++ union(只支持基础类型)](https://blog.csdn.net/wangeen/article/details/8439306)、[c++11中，含有类类型的union](https://blog.csdn.net/ml422524/article/details/77941546)、**[c++ - 关于 C++ 非POD联合的c++11问题](https://kb.kutu66.com/others/post_12551674)**、[c++中POD类型和non-POD类型](https://www.cnblogs.com/time-is-life/p/9629111.html)、[C++11非受限联合体（union）](http://c.biancheng.net/view/7165.html)

72. **C++垃圾回收机制**

    [C++中垃圾回收机制中几种经典的垃圾回收算法](https://blog.csdn.net/u012611878/article/details/78947267)、[C++垃圾回收机制](https://www.cnblogs.com/blfshiye/p/3764059.html)、[C++为什么不加入垃圾回收机制](https://blog.csdn.net/yeahhook/article/details/6796242)、[C/C++中几种经典的垃圾回收算法](https://www.cnblogs.com/lidabo/p/3487045.html)、[什么情况下 C++ 需要垃圾处理机制？](https://www.zhihu.com/question/308638046)

73. 浮点数怎么表示？大概说了下ieee754,浮点数表示范围连续吗？为什么不连续？

    [浮点数的表示范围](http://www.cppblog.com/jianjianxiaole/articles/float.html)

74. `volatile`

    [C/C++ Volatile关键词深度剖析](https://www.cnblogs.com/god-of-death/p/7852394.html)、[C++中volatile的作用](https://blog.csdn.net/weixin_41656968/article/details/80958973)

75. **那如何实现对一个常量进行引用**

76. **vector底层原理，resize和reserve方法原理**

77. **C++中异常处理中try-catch的实现机制,问假设一个try后有多个catch，catch的异常类型相同，是怎么匹配到对应的catch的。**

    [C++异常处理（try catch throw）完全攻略](http://c.biancheng.net/view/422.html)、[C++异常机制的实现方式和开销分析](http://www.baiy.cn/doc/cpp/inside_exception.htm)、[异常](http://www.baiy.cn/doc/cpp/index.htm#%E4%BB%A3%E7%A0%81%E9%A3%8E%E6%A0%BC%E4%B8%8E%E7%89%88%E5%BC%8F_%E5%BC%82%E5%B8%B8)、[C++异常处理机制](https://blog.csdn.net/caihaitao2000/article/details/79827857)

78. **C++中组合、继承的机制了解吗？**

79. **虚函数指针存放在内存中什么位置**

80. **调用`malloc`的时候要求 8 bytes 内存对齐，怎么处理？**

    [malloc(malloc在32位编译系统中分配的地址会8字节对齐，64为编译系统中会8或者16字节对齐)](https://blog.csdn.net/sweetfather/article/details/79798962)、[malloc内存分配字节对齐问题](https://blog.csdn.net/shemangui/article/details/50459102?locationNum=1)、[malloc分配内存进行对齐的操作](https://www.cnblogs.com/sigma0/p/10837760.html)、[用malloc分配内存的字节对齐，指针对齐](https://www.douban.com/note/643194488/)、[malloc会保证内存对齐吗？](http://cn.voidcc.com/question/p-cdlgkjum-sm.html)

81. **介绍C++内存配置器**

    [c++ STL（六 空间配置器、内存配置器）](https://www.cnblogs.com/xietianjiao/p/12344339.html)、[c++ stl 内存配置器](https://www.cnblogs.com/xietianjiao/p/12344098.html)、[**STL库的内存配置器（allocator）**](https://www.cppentry.com/bencandy.php?fid=49&id=149890)

82. 实现`lower_bound`

83. 考虑如下代码
    ```c++
    class A {
        virtual void foo() = 0;
    };
    class B : public  A{
    };
    ```

    - `sizeof(A)`在 32 位的机器上会出现什么问题？
      `sizeof(A)=4`

    - 使用`class B`的话，后续可能会出现什么问题？（析构函数不是虚函数）

    - 编译器为`class B`生成了哪些函数？有右值版本的函数吗？

84. **下面例子中，`std::move`失效的情况，即没有调用右值版本的`push_back()`。（A是内置类型或者A没有右值版本的拷贝构造函数）**

    在c++11之后已经有了右值版本的`push_back()`

    ```c++
    vector<A> a;
    a.push_back(A());
    ```

85. **怎么让一个类获取到自己的`shared_ptr`？主要考察`enable_shared_from_this`。**

    在`class`内部,获得`this`指针的`shared_ptr`,需要`class`继承`std::enable_shared_from_this`

    、[std::enable_shared_from_this 有什么意义？](https://www.zhihu.com/question/30957800)、[C++智能指针的enable_shared_from_this和shared_from_this机制](https://blog.csdn.net/QIANGWEIYUAN/article/details/88973735)、[C++11新特性之十：enable_shared_from_this](https://blog.csdn.net/caoshangpa/article/details/79392878)、

86. **sizeof一个空类的大小，为什么是1，以及衍生的一些判断类大小以及为什么；**

    [空类的sizeof为1，为什么？](https://blog.csdn.net/cy_cai/article/details/52979402)

87. **虚函数表指针通常存储在类对象的哪个位置?为什么**

88. **`std::move`失效**

    [C++ lambda 内 std::move 失效问题的思考](https://www.cyhone.com/articles/why-move-no-work-in-lambda/)如果`std::move`的参数是一个常量引用，则会造成失效。

---

# 操作系统

## 内存管理

1. malloc和mmap的底层实现？malloc分配的是什么？

2. Linux进程地址空间布局？（这里又问道虚拟内存和分页、页表这些东西）

3. 页表是什么

4. 堆上的对象和栈上的对象有什么分别？

5. 内存分配算法了解吗，比如说给你一个内存池，碎片和效率，策略

6. 内存泄漏怎么规避呢？

7. 操作系统的内存分配

8.  C++ 内存泄漏和内存越界有什么区别？内存泄漏怎么预防

9. 栈空间分配

10. 内存中堆和栈那一种结构快?为什么栈更快?

    栈快,系统控制释放，内存连续，无碎片

11. 堆区栈区的区别？--》C++的内存分布

12. 用户态与内核态的区别？（为什么要有内核态）

13. Mmap在用户区还是内核区？为什么快速？

14. 检测内存泄漏

15. 动态内存申请方式？会不会失败？失败会怎么样？

16. 栈里面放的一般都有什么，栈和堆了解吗？

17. 访问NULL指针为什么会报错？编译期报错还是运行期报错？编译器会编译这个指令，所以应该是运行期报错，0号地址也是地址，如何实现这个访问0号地址报错这个机制？内存是如何寻址的？
    这里一开始只想到了寻址方式，没推测出答案，后来查发现他想说的其实应该是地址转换，这里我后来查到好像是0号地址在地址转换的过程中会把0号地址映射到一个用户没有权限访问的一个页中，然后每次在这个时候报错。

18. 如果malloc 1G的内存，将其512M位置的地方赋值为1，此时实际分配的物理内存会是512M吗？

19. C++内存分配堆栈在内存中的大小,使用malloc函数，在32位机器上1G的物理内存能获取到的内存大小。

20. 用户态到内核态的切换，两者怎么进行通信

    [用户态与内核态之间的切换](https://blog.csdn.net/u012333003/article/details/29384807)、[内核态和用户态的区别 zz](https://www.cnblogs.com/viviwind/archive/2012/09/22/2698450.html)、[Linux用户态与内核态通信的几种方式](https://cloud.tencent.com/developer/article/1454965)、[Linux探秘之用户态与内核态](https://www.cnblogs.com/bakari/p/5520860.html)、[用户态/内核态/线程/协程](https://blog.nowcoder.net/n/5541fdad534649c2933937a7af0be1ec)

## 进程

1. **进程调度的方式有哪些？Linux中进程调度的方式？**

    [【Linux】Linux的进程调度](https://blog.csdn.net/qq_38410730/article/details/81253686)、[【Linux】几种常见的进程调度](https://blog.csdn.net/qq_39487033/article/details/83097374)、[Linux进程调度原理](https://www.cnblogs.com/zhaoyl/archive/2012/09/04/2671156.html)、[操作系统中的进程调度策略有哪几种](https://blog.csdn.net/qq_35642036/article/details/82809812)、[Linux常见的进程调度算法](https://www.cnblogs.com/alantu2018/p/8460451.html)

2. **单进程与IO复用？**

    [IO复用、多进程和多线程三种并发编程模型](https://blog.csdn.net/wan_hust/article/details/38441455)、[I/O复用的理解](https://blog.csdn.net/xd_rbt_/article/details/80287959)、[IO模式和IO多路复用详解](https://www.cnblogs.com/lifei01/p/10733532.html)、[一文看懂IO多路复用](https://zhuanlan.zhihu.com/p/115220699)、[Linux IO模式及 select、poll、epoll详解（含部分实例源码）](https://zhuanlan.zhihu.com/p/148972109)、[Linux I/O复用中select poll epoll模型的介绍及其优缺点的比较](https://zhuanlan.zhihu.com/p/141447239)、[如果这篇文章说不清epoll的本质，那就过来掐死我吧！](https://zhuanlan.zhihu.com/p/63179839)

3. **下面代码打印输出？（写出四组）**

    ```c++
    #include <sys/types.h>
    #include <unistd.h>
    
    pid_t pid1;
    pid_t pid2;
    
    pid1 = fork();
    pid2 = fork();
    
    cout << "cur pid:"<<getpid() << ",pid1:" << pid1 << ",pid2:" << pid2 << endl;
    ```

    

4. **僵尸进程和孤儿进程？**

    [操作系统-进程状态转换图](https://blog.csdn.net/sofia_fhx/article/details/88758007)、[进程的状态转换](https://www.cnblogs.com/lrhya/p/10636011.html)、[僵尸进程与孤儿进程详解](https://blog.csdn.net/weixin_41050155/article/details/81945208)、[孤儿进程和僵尸进程的区别](https://blog.csdn.net/a13568hki/article/details/103851388)

5. **一个进程能不能绑定到一个cpu？系统调用接口是什么？线程呢？**

    [如何将一个进程（线程）绑定到一个固定的CPU核上？](https://blog.csdn.net/sangyongjia/article/details/50725734)、[Linux进程或线程绑定到CPU](https://www.cnblogs.com/swey/p/4469722.html)、[管理处理器的亲和性（affinity）](https://www.ibm.com/developerworks/cn/linux/l-affinity.html)、[一个进程能不能在多个核上跑？](https://www.zhihu.com/question/31683094?sort=created)

6. **fork、死锁**

    [百度百科fork](https://baike.baidu.com/item/fork/7143171?fr=aladdin)、[fork()函数详解](https://www.cnblogs.com/love-jelly-pig/p/8471206.html)、[浅析fork()和底层实现](https://www.cnblogs.com/tp-16b/p/9005079.html)

7. **对进程调度的理解，用来解决什么问题？进程调度的策略有哪些（调度算法）？我们用的电脑用的什么算法（时间片轮转法）？**

    在计算机系统中，可能同时有数百个批处理作业存放在磁盘的作业队列中，或者有数百个终端与主机相连接，这样一来内存和处理器等资源便供不应求。如何从这些作业中挑选作业进入主存运行、如何在进程之间分配处理器时间，无疑是操作系统资源管理中的一个重要问题。因此引入处理器调度用来完成涉及处理器分配的工作，而其中的低级调度，执行分配CPU 的程序即分派程序(dispatcher)，它是操作系统最为核心的部分，执行十分频繁，低级调度策略优劣直接影响到整个系统的性能。引入的目的是按照某种原则决定就绪队列中的哪个进程或内核级线程能获得处理器，并将处理器出让给它进行工作。

    以上引用[为什么要引入进程调度,引入进程调度的目的是什么?](https://zhidao.baidu.com/question/344015549.html)

    参考[如何理解进程调度?](https://www.douban.com/note/709879948/)、[进程调度总结](https://www.jianshu.com/p/6e1fa5cf62cb)、[处理机调度：调度的概念、时机、切换、过程以及调度方式和基本准则](http://c.biancheng.net/cpp/html/2594.html)

8. **进程通信的机制有哪些？（IPC）**

    信号，管道（命名管道FIFO，无名管道PIPE）、共享内存、信号量、消息队列、

9. 可重入函数：只了解strork

10. **父进程子进程共享什么？**

    [父子进程共享内容](https://blog.csdn.net/zhuoyue01/article/details/105976937/)

11. **子进程的生命周期受什么影响？**

      [进程生命周期](https://blog.csdn.net/q1n2hen/article/details/77996396)、[进程的生命周期](https://blog.csdn.net/xiaojunling/article/details/81476257)

12. **进程有几种状态？执行到就绪的转换情况？执行到阻塞的转换情况？**

13. 点击鼠标，内存发 生了什么？

14. **用锁的时候发生竞争会把进程陷入到内核态还是保留在用户态，为什么**

      [用户态切换到内核态主要有三种方式](https://blog.csdn.net/lengye7/article/details/88392407)、[对内核,内核线程,用户线程,用户态,内核态,进程等理解](https://blog.csdn.net/gd359353793/article/details/86715099)

15. **饿汉模式；懒汉模式；线程安全的写法；**

      [单利模式（饿汉模式，懒汉模式）线程安全与解决问题](https://www.cnblogs.com/da-peng/p/8278714.html)、[JavaWeb~用饿汉模式、懒汉模式(高性能线程安全版)分别实现单例模式](https://blog.csdn.net/Shangxingya/article/details/106715371)

16. **一个进程可以打开的fd有限，到并发量很大的时候，fd耗尽了，怎么办？**

      [通过FD耗尽实验谈谈使用HttpClient的正确姿势](https://blog.csdn.net/yunqiinsight/article/details/86468967)、[为什么select打开的FD数量有限制，而poll、epoll等打开的FD数量没有限制？](https://www.zhihu.com/question/37219281?sort=created)

17. **惊群模式**

      [Linux惊群效应详解（最详细的了吧）](https://blog.csdn.net/lyztyycode/article/details/78648798?utm_source=blogxgwz2)、[Linux网络编程“惊群”问题总结](https://www.cnblogs.com/Anker/p/7071849.html)、[Linux网络编程“惊群”问题](https://www.jianshu.com/p/9d0899c9ec89)、[什么是惊群，如何有效避免惊群?](https://blog.csdn.net/thlzjfefe/article/details/97495073)、[解决多进程模式下引起的“惊群”效应](https://zhuanlan.zhihu.com/p/42882130)、[[框架]高并发中的惊群效应](https://blog.csdn.net/second60/article/details/81252106)

18. **进程切换上下文细节**

     [进程切换（进程上下文和中断上下文）详解](https://blog.csdn.net/yapian8/article/details/41809837)、[进程上下文切换分析](https://www.cnblogs.com/tjyuanxi/p/9314810.html)、[进程/线程上下文切换会用掉你多少CPU？](https://www.jianshu.com/p/ebf1f832694c)

19. **不同进程是如何保证使用的内存互不影响**

     多进程的执行通过内存保护实现互不干扰，如页式管理中有页地址越界保护，段式管理中有段地址越界保护。

20. **进程挂了共享内存是否还存在，为什么？**（不知道，结束之后百度：进程间通信使用的数据结构:管道、socket、共享内存、消息队列、信号量等，是属于内核级的，一旦创建后就由内核管理，若进程不对其主动释放，那么这些变量会一直存在，除非重启系统。）

     [Linux进程间通信之共享内存](https://www.cnblogs.com/xcywt/p/5128430.html)、[进程间通信——IPC之共享内存](https://www.cnblogs.com/tp-16b/p/8987697.html)、[多进程与多线程的优劣 与 共享内存的同步问题](http://blog.chinaunix.net/uid-27164517-id-3319185.html)

21. 父进程如何fork一个子进程的，具体过程,父进程如何fork一个子进程，父进程内部的代码段、数据需要在拷贝一份给子进程吗？如果不拷贝的话，共用的话，内核如何保证他们不会出问题？

     不会立即复制，在最开始，是公用代码段和数据段，当需要进行写入的时候才拷贝。

     [fork创建进程过程（底层实现） 和 写实拷贝](https://blog.csdn.net/qq_37924084/article/details/78299352)、

22. 系统调用的最小单元是什么？线程相对于进程，独占了什么？

      [深入理解进程和线程](https://www.cnblogs.com/tiankong101/p/4229584.html)、[【面试高频问题】线程、进程、协程](https://zhuanlan.zhihu.com/p/70256971)、[内核线程、轻量级进程、用户线程三种线程概念解惑（线程≠轻量级进程）](https://blog.csdn.net/gatieme/article/details/51481863)、[进程、线程及进程调度算法](https://www.jianshu.com/p/84957c2644d8)、

## 线程

1. 线程池如何开启一个新线程？

2. 线程池

    [线程池的C++实现](https://zhuanlan.zhihu.com/p/61464921)、[C++11实现线程池](https://blog.csdn.net/u012234115/article/details/89856320)、[【C/C++】 Pthread线程池](https://zhuanlan.zhihu.com/p/88896082)、[用 C++ 写线程池是怎样一种体验？](https://www.zhihu.com/question/27908489/answer/355105668)

3.  线程池初始化的步骤？

4. 线程池中线程状态？

5. 主线程与IO线程之间描述符传递？

     [Linux 文件描述符详解](https://www.cnblogs.com/alan666/p/8311890.html)、[Linux文件描述符到底是什么？](http://c.biancheng.net/view/3066.html)、[Linux文件描述符--就这？](https://zhuanlan.zhihu.com/p/134036259)、[linux中的文件描述符(file descriptor)和文件](https://www.jianshu.com/p/504a53c30c17)

6. `pthread_detach`和`pthread_join`的作用？

    [pthread_join和pthread_detach的作用](https://blog.csdn.net/sinat_29844779/article/details/72490111)、[pthread_join和pthread_detach的用法](https://www.cnblogs.com/fnlingnzb-learner/p/6959285.html)

7. 进程线程区别、进程/线程通信

8. 锁相关了解吗？线程安全怎么办？（回答可重入函数，锁）

9. 线程、进程和协程

    [什么是协程 ？](https://juejin.im/post/5d5df6b35188252ae10bdf42)、[协程](https://zh.wikipedia.org/wiki/%E5%8D%8F%E7%A8%8B)、[coroutine协程详解](https://www.jianshu.com/p/2782f8c49b2a)、[协程的好处有哪些？](https://www.zhihu.com/question/20511233)、[协程简介](https://hanfeng.ink/post/coroutine/)

10. 死锁的产生条件，怎样预防死锁

     [死锁的四个必要条件？如何避免与预防死锁？](https://www.cnblogs.com/bopo/p/9228834.html)

11. 一个进程里面有很多线程，设计一个机制，可以知道哪些线程在工作，哪些出了问题

12. 为什么用多线程，apache为什么用多进程，nginx为什么用多线程

13. 介绍一下几种锁（互斥、信号量、条件变量、读写锁、自旋锁）

14. 运用多线程有什么要注意的吗？（join回收子线程）还有啥？

15. 子线程正常死亡与其他线程cancel这个子线程（被动死亡）有什么区别？（不知道）

     [linux下pthread_cancel无法取消线程的原因](https://blog.csdn.net/qingzhuyuxian/article/details/81019173)、[线程pthread_cancel()函数](https://blog.csdn.net/u011878172/article/details/79510824)、[线程取消(pthread_cancel)](https://www.cnblogs.com/lijunamneg/archive/2013/01/25/2877211.html)、[5、线程终止方式：pthread_cleanup_push/pthread_cleanup_pop()](https://www.cnblogs.com/mydomain/archive/2011/08/15/2139826.html)

16. 线程切换上下文细节

17. **线程共享一块内存，进程不共享，线程全都共享吗**

     [linux线程共享和进程内存的关系](https://blog.csdn.net/yuanbinquan/article/details/41699491)、[同一进程中的线程究竟共享哪些资源](https://www.cnblogs.com/baoendemao/p/3804677.html)

18. 对比I/O多路复用和多线程

## 文件系统

1. 阻塞和非阻塞？同步和异步？

2. 五种IO模型？

    [五种IO模型（详解+形象例子说明）](https://blog.csdn.net/ZWE7616175/article/details/80591587)、[IO概念和五种IO模型](https://www.cnblogs.com/shengguorui/p/11949282.html)、[五种IO模型透彻分析](https://www.cnblogs.com/f-ck-need-u/p/7624733.html)

3. select和epoll有什么区别？epoll的LT和ET模式？LT和ET适用场景？

    [select、poll、epoll的区别以及epoll的两种模式（LT、ET）以及实现](https://blog.csdn.net/qq_41038824/article/details/88537640)、[epoll两种类型ET和LT区别(结合实际例子)](https://blog.csdn.net/cws1214/article/details/47662705)、[I/O多路复用之select、epoll的实现和区别 ，ET与LT模式](https://blog.csdn.net/qingcunsuiyue/article/details/66477777)

4. select/poll/epoll、虚拟内存、用户态内核态；

5. Linux中，文件名存在哪里？

    [Linux 中文件名存在哪里？](https://blog.csdn.net/wdkirchhoff/article/details/44205143)、[Linux文件名存在哪儿](https://blog.csdn.net/pugongying_95/article/details/97295843)

6. 文件索引有哪些？

7. select和epoll有没有最大描述符限制，epoll的限制体现在哪？epoll底层用了哪些数据结构，红黑树和链表分别是用来干啥的，红黑树的key是什么？

    [epoll“传说中”的性能](https://blog.csdn.net/historyasamirror/article/details/5833259)、[Linux下实现epoll服务器和缺点总结](https://blog.csdn.net/lishitao_578/article/details/73810623)、

8. 从磁盘读取数据到CPU中间经过的介质

    硬盘数据--->硬盘缓存---->内存---->CPU缓存--->CPU寄存器与运算器

9. linux下可执行文件组织机制(内存布局,.text,.bss,.data组织方式等）。

10.  IOCP

11. linux系统怎么区别文件类型

    [linux操作系统文件类型有哪几种,有什么区别?](https://zhidao.baidu.com/question/511065836.html)、[Linux系统目录结构及文件类型区分](https://www.cnblogs.com/miaotianjie/p/8587650.html)、

12. 五种io模型

    同步IO：阻塞IO、非阻塞IO、IO多路复用、信号驱动IO；异步IO

13. 什么情况下epoll效率会比select\poll低,epoll底层的实现原理，如何保证大部分情况下效率优于select

14. 操作系统读写操作，写文件，read的三种模式，写成功是怎样的，什么情况下写操作会失败

15. [Open/Read/Write和Fopen/Fread/Fwrite的区别](https://www.cnblogs.com/ldp-web/archive/2011/10/21/2220180.html)、[多进程同时写一个文件会怎样？（待修订）](https://blog.csdn.net/yangbodong22011/article/details/63064166/)

## 中断

1. 中断的上半部与下半部的区别，什么是中断时延

## 组件

1. 如何实现日志系统？
   - 日志数据生成过快怎么解决？
2. 定时器实现？
3. 什么是实时操作系统，`如何实现实时`，`与分时操作系统的区别`。`如何实现优先级反转`

---

# 数据库

1. k-v数据库如果get有百分之80的数据都不命中有什么好的处理方法？
2. MySQL
3. sql查询太慢如何优化
4. mysql的存储引擎？两个的区别？myisam哪些环节执行效率会更高？表比较小，或者有大量的select语句
5. 部署过mysql吗，主备复制安装过吗？主备复制有哪些方式？有什么区别？
6. nosql了解过吗？关于数据库听说过哪些？
7. mysql的索引结构：
   myisam，innodb不同的索引结构
   索引的存储结构（数据结构）
8. 非主键索引？什么是覆盖索引？事务隔离级别？别介绍一下主要区别和应用场景？
9. 数据库里面的死锁介绍一下？（介绍一个具体例子）什么是死锁，什么情况下产生死锁？
10. innodb和myisam区别？为什么查询比较多的时候为什么用myisam？存储结构不同，mysiam索引和数据不在一起
11. redis有没有做持久化，redis宕掉会怎么样，怼我说你只是用redis做了一个没有质量保证的消息队列而已
12. 数据库常用优化方法，数据库四大隔离级别
13. SQLite和MySQL的区别
14. redis的高可用，主从、哨兵、集群，主从的选举过程
15. mysql高可用，主从，最新版本？mysql主从同步是异步的吗？有同步的吗？半同步复制
16. 关系型数据库是什么
17. b+树说说。为什么数据库用b+树不用红黑树？
18. 为什么索引不宜太多？索引太多，文件过大。还有第二点没答出来，更新数据的时候索引也要更新，耗时。
19. 数据库cpu跟不上请求频率，应该如何优化？
20. 无锁队列之类
21. 数据库问了好多，问了好多底层的东西；数据库结构是hash结构还是树形，为什么?数据库有哪些优化？
22. map的底层实现？zset的底层实现？为什么zset不使用红黑树作为其底层实现？为什么map不使用跳表作为其底层实现？
23. MYSQL中实现网页翻页的话怎样写SQL语句？
24. sql注入了解吗？

---

# 计算机网络

1. Connect 函数与非阻塞？

2. Accept函数与三次握手关系？

    Accept函数是返回已经三次握手成功的socket

3. TIME_WAIT时长？为什么？如何取消？

    [详解 TCP 连接的“ 三次握手 ”与“ 四次挥手 ”](https://baijiahao.baidu.com/s?id=1654225744653405133&wfr=spider&for=pc)、[详解TCP连接的“三次握手”与“四次挥手”(上)](https://www.cnblogs.com/AhuntSun-blog/p/12028636.html)、[详解TCP连接的“三次握手”与“四次挥手”(下)](https://www.cnblogs.com/AhuntSun-blog/p/12037852.html)

4. 报文的最大生存期与那些因素有关？

    [MSL(Maximum Segment Lifetime)报文最大生存时间](https://blog.csdn.net/weixin_42764391/article/details/99820386)、[网络术语MSL/TTL/RTT](https://blog.csdn.net/u013074465/article/details/45097183)

5. tcp的握手挥手过程？（详细）tcp为什么要连接？tcp建立连接这里你是怎么理解的？

6. 半连接队列？全连接队列？

    [关于TCP 半连接队列和全连接队列](http://jm.taobao.org/2017/05/25/525-1/)、[从一次 Connection Reset 说起，TCP 半连接队列与全连接队列](https://cjting.me/2019/08/28/tcp-queue/)、[TCP的全连接和半连接队列](https://juejin.im/post/5e01a5886fb9a016510db122)、[TCP半连接队列和全连接队列](https://blog.csdn.net/yangguosb/article/details/90644683)、[TCP 半连接队列和全连接队列满了会发生什么？又该如何应对？](https://www.cnblogs.com/xiaolincoding/p/12995358.html)

7. tcp流量控制和拥塞控制？输层发生拥塞，在应用层的表现是什么

    [TCP流量控制、拥塞控制](https://zhuanlan.zhihu.com/p/37379780)、[TCP流量控制与拥塞控制](https://andrewpqc.github.io/2018/07/21/tcp-flow-control-and-congestion-control/)、[TCP的流量控制和拥塞控制](https://blog.csdn.net/liaoqianwen123/article/details/25429143)、[tcp流量控制，拥塞控制和可靠传输](https://troywu0.gitbooks.io/spark/content/tcp%E6%B5%81%E9%87%8F%E6%8E%A7%E5%88%B6%EF%BC%8C%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6%E5%92%8C%E5%8F%AF%E9%9D%A0%E4%BC%A0%E8%BE%93.html)

8. 网络编程里的reactor模型介绍下

9. time_wait状态？为什么是2msl？

10. 有很多close_wait怎么解决？

    [TCP CLOSE_WAIT 过多解决方案](https://blog.csdn.net/wwd0501/article/details/78674170)、[一次服务端大量CLOSE_WAIT问题的解决](https://blog.csdn.net/yu616568/article/details/44677985)、[线上大量CLOSE_WAIT的原因深入分析](https://juejin.im/post/5c0cf1ed6fb9a04a08217fcc)、[CLOSE_WAIT连接过多的现象分析与处理](https://www.zybuluo.com/zhongdao/note/1450198)

11. udp为什么是不可靠的？bind和connect对于udp的作用是什么？

12. NAT是什么？底层实现原理？

13. 对于使用tcp通信的两端，如果client已经退出，此时服务端继续send会出现什么问题？这个当然就要扯到SIGPIPE信号了？

15. DDOS攻击原理

16. http/https协议，cookie/session、输入url发生了什么；

17. https，非对称加密，通信中怎么弄？为什么通信前用非对称加密，通信中要用对称？为什么不一直用非对称？

18. xss了解吗？

20. TCP与UDP什么关系？

21. tls和ssl什么区别？

22. TCP怎样发现数据丢了，丢了会怎样处理？TCP的全双工怎么理解？TCP一端还在发但是另一端已经关闭了怎么办，就是我知道对面已经关闭连接了但是我还要向他发数据，在应用层该怎么处理？

23. 什么叫面向流、面向报文？

24. UDP发送到应用层的一条数据有可能会被拆分成多个数据包吗？

27. http1.0/1.1 和 http2.0的区别?post和get的区别?

28. dns 的解析过程，DHCP

29. 大端和小端存储概念；

30. 使用sendto发送大小为1024B的UDP报文，使用recvfrom每次接收64B数据，一个while循环是否能取得所有数据？

     [关于udp socket recvfrom函数的一个易错问题： 如果应用程序指定的接收长度不够怎么办？](https://blog.csdn.net/stpeace/article/details/73893884)、[告知你不为人知的 UDP：疑难杂症和使用](https://cloud.tencent.com/developer/article/1004554)

31. 路由表的问题

32. 网络各层校验用的什么？ ARP协议？

33. 问linux网络驱动的实现步骤

34. 网络故障如何查看网络状态？

35. 实现一个网络库有哪些要点？

36. 硬件地址和ip地址的区别。

33. 网络协议栈的理解

      [理解 Linux 网络栈（1）：Linux 网络协议栈简单总结](https://www.cnblogs.com/sammyliu/p/5225623.html)、[如何学习 Linux 内核网络协议栈](https://segmentfault.com/a/1190000021227338)、[Linux 网络栈剖析](https://www.ibm.com/developerworks/cn/linux/l-linux-networking-stack/)、[重头梳理网络协议栈](https://www.jianshu.com/p/75c7f1a0a4e7)、[了解linux网络协议栈（四）——协议栈实现](https://blog.csdn.net/lee244868149/article/details/71248999)、[Linux网络栈](https://www.cnblogs.com/agilestyle/p/11394930.html)、[协议栈](https://www.zhihu.com/topic/20070536/hot)

34. 服务器启动到接受连接的整过过程锁调用的函数

35. 出现丢包的情况，服务端发现重复ack，在socket层面的表现是？可以通过什么函数获取到对应的信息？

36. 有没有了解其他开源网络库？了解协程网络库吗？

37. 网络抓包用什么工具？wireshark，tcpdump

38. arp协议是那一层的？arp协议是干什么的？

     arp是属于网络层，用于将ip得知解析成mac地址

39. icmp协议分别位于哪层

     网络层（ip层）

40. traceroute原理

     tranceroute使用IPv4的TTL字段或IPv6的跳线字段以及两种ICMP消息。它一开始向目的地发送一个TTL（或跳限）为1的UDP数据报。这个数据报导致第一跳路由器返回一个ICMP "time exceeded in transmit"（传输中超时）错误。接着它每递增TTL一次发送一个UDP数据报，从而逐步确定下一跳路由器。当某个UDP数据报达到最终目的地时，目标是由这个这个主机返回一个ICMP "port unreachable"（端口不可达）错误。这个目标通过向一个随机选取的（但愿）未被目的主机使用的端口发送UDP数据报得以实现。

41. 集群和分布式有什么区别

42. HTML怎么优化

43. cookies 和session区别和作用

44. 黏包问题

     [TCP粘包问题分析和解决（全）](https://blog.csdn.net/tiandijun/article/details/41961785)、[怎么解决TCP网络传输「粘包」问题？](https://www.zhihu.com/question/20210025)、[Socket 中粘包问题浅析及其解决方案](http://www.hchstudio.cn/article/2018/d5b3/)、[为什么 TCP 协议有粘包问题](https://draveness.me/whys-the-design-tcp-message-frame/)、[TCP粘包/拆包问题](https://www.cnblogs.com/wade-luffy/p/6165671.html)、[Socket编程（4）TCP粘包问题及解决方案](https://www.cnblogs.com/QG-whz/p/5537447.html)

45. UDP可靠传输实现

46. 数据包mtu(最大传输单元)

     [TCP/IP协议：最大传输单元MTU 和 最大分段大小MSS (TCP的分段和IP的分片)](https://blog.csdn.net/qq_26093511/article/details/78739198)、[MTU最大传输单元](https://www.jianshu.com/p/678f5e79dd71)、

47. UDP广播IP段？

48. socket中 bind() accept()之后的状态

49. 如果有很多从客户端发来的请求，结果大量的失败，分析原因
      我说了网络，I/O，后来他又问从操作系统层面考虑，我说是不是socket占用了大量的端口号，然后又问端口号的最大个数

50. 数据从应用层  到  传输层  到  IP层 到  链路层，每一层涉及的具体的操作有哪些？

51. TCP UDP IP首部多大

     [TCP、UDP数据包大小的限制](https://blog.csdn.net/caoshangpa/article/details/51530685)、[IP、TCP、UDP首部详解](https://blog.csdn.net/zhangliangzi/article/details/52554439)

52. 服务器方，如果两个进程同时占用80端口，客户端的HTTP报文怎么处理？会报错吗？

53. 如何判断机器是大端存储还是小端存储，如何将大端转换成小端？

54. 路由协议 RIP，netstat 显示哪些东西

---

# 数据结构与算法

1. 斐波那契数列的非递归写法？
2. 一个整数数组，可能是降序或升序，也可能是先升序再降序，求最大值？（几秒后就让我赶紧说思路）
3. 写代码：单链表，求中部的N个节点的头节点和尾节点？（需要写测试用例）
4. 一致性hash？
5. 判断输入的数据是不是合法分float类型数据（自动机）
6. 手撕平衡二叉树
7. LRU实现
8. 了解哪些数据结构？
9. 说一下二分查找的思想，二分查找用递归实现，递归有什么缺点？
10. 用过hash表吗？我说只在unoedered_map容器里用过，就没再问
11. 给一个整数，得到它的开根号的值。怎么实现这个函数？
12. 快排什么时候会退化？随机法；二路快排
13. 哈希了解过吗？设计一个哈希算法，考虑什么？（哈希冲突少，其他的还有吗）怎么让哈希算法冲突更少，有没有什么策略？
14. 为什么数据规模较小的时候可以采用插入排序？
15. 为什么在实现一个快排的时候，比如STL，实现快排，然后到一个规模的时候下选择插入，有什么考量的点
16. 了解过哪些查询算法？
17. 如何统计一篇文章中单词的频次。
18.  查找一个关键字的时间复杂度？堆排序的时间复杂度？有没有比O(logn)更小时间复杂的的查找方法？
19. B 树是什么？平衡二叉树是什么？
20. 快排是不是稳定的，有哪些排序方法是稳定的
21. B+树原理，红黑树原理
22. 合并两个二叉树 算法思路
23. 最小栈问题；
24. 如何从100w个无序数中获取第K大的数？
25. 红黑树最差旋转几次
26. 红黑树和AVL数树区别?
27. 设计哈希表，如何扩容
28. 海量数据排序该怎么做
29. 既然计数排序时间复杂度低，为什么它并不常用
30. 海量数据找中位数
31. kmp算法以及复杂度
32. LFU逻辑上如何实现 要求访问是O(1)的
33. 红黑树和AVL树，红黑树的平衡因子？？？
34. 最小生成树算法？图的广度优先遍历用到的是什么数据结构？

---

# 设计模式

1. 学过什么设计模式，简述？了解设计原则吗?
2. 线程安全的单例模型怎么实现?

---

# 编译原理

1. 动态编译和静态编译
2. 编译时链接有几种方式: 静态链接和动态链接的区别?
3. 动态链接库和静态链接库的链接时机，预处理、编译还是其他阶段？
4. 如果动态链接库A，动态链接库B，里面都有一个名为function的函数，函数名相同，函数具体定义内容不同，程序链接阶段，怎么才能确定链接哪个函数。

---

# Linux

1. linux查看端口命令：`lsof -i`
2. gdb用过吗，如何查看调用栈（bt，backtrace)
3. 常用的linux命令，top,netstat,df,free,strace,pstack
4. cpu占用过高了，如何排查？（不知道）
5. 程序crash了，如何排查？（不知道）

---

# 嵌入式

1. 问PSRAM、SDRAM、DDR、DDR2的时序特性？I2C触摸屏芯片与CPU的数据传输流程？画出相关图例？
2. uboot启动时为什么要关闭中断，Icache有什么好处。

---

# 其他

1. 微服务的架构、框架的实现、服务治理、分布式一致性
2. 日志监控系统，考察架构，ELK的相关知识，重点描述Elasticsearch的一些架构原理，比如倒排索引的原理；
3. 问消息队列的相关知识，接触过Kafka，问了Kafka中消息可以被多个消费者消费吗？；以及选举机制和HW机制；
4. 数据库和Redis相关问题，数据库考察索引的原理以及几种事务的区别。Redis问数据结构，延时队列如何实现，分布式锁原理；
5. 负载均衡算法；
6. ngnix如何做限流；
7. 四层LVS和七层Ngnix的区别；
8. 秒杀系统的如何设计，分接入层、接口层、消息队列层、逻辑层四个方面讲解，接入层可以做服务治理相关事情，接口层做抢购开关、黑白名单、随机拒绝等处理，逻辑层具体抢购逻辑实现，涉及到redis分布式锁以及DB和Redis的一致性问题。从秒杀系统还引申出分布式事务的几种实现，二段式、三段式、补偿型(TCC)、基于可靠消息服务的消息队列实现。重点讨论了这几种的实现和区别，要求画出基于可靠消息服务的消息队列实现分布式事务的架构图，以及上游服务和下游服务如何保证消息可靠性和一致性。
9. 考察服务治理，服务限流算法，服务降级的指标和恢复指标，服务熔断。
10. 共享桌面用你熟悉的任意画图工具画项目架构图？
11. 项目中遇到缓存穿透问题怎么处理？
12. ZooKeeper
13. 如何调试服务器？
14. rpc，taf框架库
15. UML
16. 给定一堆url，用于查找，怎么做？
17. 事务管理（ACID）
18. 浏览器输入一个URL会经历一些什么步骤？
19. 为啥第二次打开页面会比第一次快？
20. 大数相乘
21. nginx的网络模型，IO多路复用
22. 状态机；行为树；
23. 每秒有2w qq号登陆，怎么找出每小时登陆次数5-10次的所有qq号(实时)？
24. cdn业务的问题，
25. 如果用户访问的缓存节点没有资源，怎么办？自然是向父节点寻找资源，父节点没有继续向父节点查找，直到找到源站。
26. 用户访问的域名和缓存节点向父节点访问的域名一样吗？
27. 十亿字符串中查找一个字符串是否存在，bitmap、布隆过滤器，失误率，如何降低
28. protobuf
29. Asio 
30. 分布式和高可用性什么关系？
31. 10TB数据找TOP100