---
title: C/C++基础合集：模板编程
layout: post
date: 2020-06-11 20:04:00 +0800
categories: 秋招
tags:
	- c/c++
	- 面经
	- 秋招
---

关于c++模板编程相关的一些面试常考知识点，许多资料基本都来源与互联网，每一部分都标明了从何处引用，与先关链接。

[TOC]

---

# 泛型编程

泛型编程（Generic Programming）最初提出时的动机很简单直接：发明一种语言机制，能够帮助实现一个通用的标准容器库。所谓通用的标准容器库，就是要能够做到，比如用一个List类存放所有可能类型的对象这样的事；泛型编程让你编写完全一般化并可重复使用的算法，其效率与针对某特定数据类型而设计的算法相同。泛型即是指具有在多种数据类型上皆可操作的含义，与模板有些相似。STL巨大，而且可以扩充，它包含很多计算机基本算法和数据结构，而且将算法与数据结构完全分离，其中算法是泛型的，不与任何特定数据结构或对象类型系在一起。

泛型编程的代表作品STL是一种高效、泛型、可交互操作的软件组件。STL以迭代器 (Iterators)和容器(Containers)为基础，是一种泛型算法(Generic Algorithms)库，容器的存在使这些算法有东西可以操作。STL包含各种泛型算法(algorithms)、泛型迭代器（iterators）、泛型容器(containers)以及函数对象(function objects)。STL并非只是一些有用组件的集合，它是描述软件组件抽象需求条件的一个正规而有条理的架构。

泛型的第一个好处是编译时的严格类型检查。这是集合框架最重要的特点。此外,泛型消除了绝大多数的类型转换。如果没有泛型，当你使用集合框架时，你不得不进行类型转换。

关于泛型的理解可以总结下面的一句话，它是把数据类型作为一种参数传递进来。

熟悉一些其它面向对象的语言的人应该知道，如Java里面这是通过在List里面存放Object引用来实现的。Java的单根继承在这里起到了关键的作用。然而单根继承对C++这样的处在语言链底层的语言却是不能承受之重。此外使用单根继承来实现通用容器也会带来效率和类型安全方面的问题，两者都与C++的理念不相吻合。

以上引用[泛型编程](https://baike.baidu.com/item/%E6%B3%9B%E5%9E%8B%E7%BC%96%E7%A8%8B)

---

# 模板介绍

## 为什么要使用模板？

模版最初的目的就是为了减少重复代码，所以模版出现的目的就是为了解放C++程序员生产力。

最初C++是没有标准库的，任何一门语言的发展都需要标准库的支持，为了让C++更强大，Bjarne Stroustrup觉得需要给C++提供一个标准库，但标准库设计需要一套统一机制来定义各种通用的容器（数据结构）和算法，并且能很好在一起配合，这就需要它们既要相对的独立，又要操作接口保持统一，而且能够很容易被别人使用(用到实际类中），同时又要保证开销尽量小（性能要好）。 Bjarne Stroustrup 提议C++需要一种机制来解决这个问题，所以就催生了模板的产生，最后经标准委员会各路专家讨论和发展，就发展成如今的模版， C++ 第一个正式的标准也加入了模板。

C++模版是一种解决方案，初心是提供参数化容器类和通用的算法（函数）。

## 什么是参数化容器类？

首先C++是可以提供OOP（面向对象）范式编程的语言，所以支持类概念，类本身就是现实中一类事物的抽象，包括状态和对应的操作，打个比喻，大多数情况下我们谈论汽车，并不是指具体某辆汽车，而是某一类汽车（某个品牌），或者某一类车型的汽车。所以我们设计汽车这个类的时候，各个汽车品牌的汽车大体框架（骨架）都差不多，都是4个轮子一个方向盘，而且操作基本上都是相同的，否则学车都要根据不同厂商汽车进行学习，所以我们可以用一个类来描述汽车的行为：

```c++
class Car {
public:
  Car(...);
      //other operations
      // ...
private:
    Tire m_tire[4];
    Wheel m_wheel;
    //other attributes
     //...
};
```

但这样设计扩展性不是很好，因为不同的品牌的车，可能方向盘形状不一样，轮胎外观不一样等等。所以要描述这些不同我们可能就会根据不同品牌去设计不同的类，这样类就会变得很多，就会产生下面的问题：

1.  代码冗余，会产生视觉复杂性，本身相似的东西比较多；
2.  用户很难通过配置去实现一辆车设计，不好定制化一个汽车；
3.  如果有其中一个属性有新的变化，就得实现一个新类，扩展代价太大。

这个时候，就希望这个类是可以参数化的（属性参数化），可以根据不同类型的参数进行属性配置，继而生成不同的类。类模板就应运而生了，类模板就是用来实现参数化的容器类。

## 什么是通用算法？

算法就是对容器的操作，对数据结构的操作，一般算法设计原则要满足KISS原则，功能尽量单一，尽量通用，才能更好和不同容器配合，有些算法属于控制类算法（比如遍历），还需要和其他算法进行配合，所以需要解决函数参数通用性问题。举个例子, 以前我们实现通用的排序函数可能是这样：

```c++
void sort (void* first, void* last, Cmp cmp);
```

这样的设计有下面一些问题：

1.  为了支持多种类型，需要采用`void*`参数，但是`void*`参数是一种类型不安全参数，在运行时候需要通过类型转换来访问数据。
2.  因为编译器不知道数据类型，那些对`void*`指针进行偏移操作（算术操作）会非常危险（GNU支持），所以操作会特别小心，这个给实现增加了复杂度。

所以要满足通用（支持各种容器），设计复杂度低，效率高，类型安全的算法，模板函数就应运而生了，模板函数就是用来实现通用算法并满足上面要求。

## 模板是怎么解决上面问题的？

C++标准委员会采用一套类似函数式语言的语法来设计C++模板，而且设计成图灵完备 (Turing-complete)（详见参考），我们可以把C++模板看成是一种新的语言，而且可以看成是函数式编程语言，只是设计依附在(借助于）C++其他基础语法上（类和函数）。

![img](https://gitee.com/zyuegege/images/raw/master/imgs/1389958-20180501140213396-545974513.png)

###  C++实现参数化类（class template）技术

1.  定义模板类，让每个模板类拥有模板签名

    模板语法：

    ```c++
    template <typename T>
    class x{
        // TODO
    };
    ```

    -   上面的模板签名可以理解成:X<typename T>; 主要包括模板参数<typename T>和模板名字X（类名）， 基本的语法可以参考《C++ Templates: The Complete Guide》，《C++ primer》等书籍。
    -   模板参数在形式上主要包括四类：
        -   **type template parameter，类型模板参数，以class或typename 标记**: 此类主要是解决朴实的参数化类的问题（上面描述的问题），也是模板设计的初衷
        -   **non-type template parameter，非类型模板参数，比如整型，布尔，枚举，指针，引用等**: 此类主要是提供给大小，长度等整型标量参数的控制，其次还提供参数算术运算能力，这些能力结合模板特化为模板提供了初始化值，条件判断，递归循环等能力，这些能力促使模板拥有图灵完备的计算能力。
        -   **template template parameter，模板参数是模板**: 此类参数需要依赖其他模板参数（作为自己的入参），然后生成新的模板参数，可以用于策略类的设计policy-base class
        -   **parameter pack，C++11的变长模板参数**: 此类参数是C++11新增的，主要的目的是支持模板参数个数的动态变化，类似函数的变参，但有自己独有语法用于定义和解析（unpack），模板变参主要用于支持参数个数变化的类和函数，比如`std::bind`，可以绑定不同函数和对应参数，惰性执行，模板变参结合`std::tuple`就可以实现

2.  在用模板类声明变量的地方，把模板实参（Arguments）（类型）带入模板类，然后按照匹配规则进行匹配，选择最佳匹配模板

    模板实参和形参类似于函数的形参和实参，模板实参只能是在编译时期确定的类型或者常量，C++17支持模板类实参推导

3.  选好模板类之后，编译器会进行模板类实例化--记带入实际参数的类型或者常量自动生成代码，然后再进行通常的编译

### C++实现模板函数（function template）技术

模板函数实现技术和模板类形式上差不多：

```c++
template<typename T>
retType  function_name(T  t);
```

其中几个关键点：

1.  函数模板的签名包括模板参数，返回值，函数名，函数参数, cv-qualifier
2.  函数模板编译顺序大致：名称查找(可能涉及参数依赖查找)->实参推导->模板实参替换(实例化,可能涉及 SFINAE)->函数重载决议->编译
3.  函数模板可以在实例化时候进行参数推导，必须知道每个模板的实参，但不必指定每个模板的实参。编译器会从函数实参推导缺失的模板实参。这发生在尝试调用函数、取函数模板地址时，和某些其他语境中
4.  函数模板在进行实例化后(template argument deduction/substitution)会进行函数重载解析（overloading resolution, 此时的函数签名不包括返回值
5.  函数模板实例化过程中，参数推导不匹配所有的模板或者同时存在多个模板实例满足，或者函数重载决议有歧义等，实例化失败
6.  为了编译函数模板调用，编译器必须在非模板重载、模板重载和模板重载的特化间决定一个无歧义最佳的模板

### 实现C++模板的几个核心技术

1.  SFINAE - Substitution failure is not an error 

    要理解这句话的关键点是failure和error在模板实例化中意义，模板实例化时候，编译器会用模板实参或者通过模板实参推导出参数类型带入可能的模板集（模板备选集合）中一个一个匹配，找到最优匹配的模板定义

    Failure：在模板集中，单个匹配失败，Error： 在模板集中，所有的匹配失败

    所以单个匹配失败，不能报错误，只有所有的匹配都失败了才报错误。

2.  模板特化

    模板特化为了支持模板类或者模板函数在特定的情况（指明模板的部分参数（偏特化）或者全部参数（完全特化））下特殊实现和优化，而这个机制给与模板某些高阶功能提供了基础，比如模板的递归（提供递归终止条件实现），模板条件判断（提供true或者false 条件实现）等。

3.  模板实参推导

    模板实参推导机制给与编译器可以通过实参去反推模板的形参，然后对模板进行实例化，具体推导规则见参考;

4.  模板计算

    模板参数支持两大类计算：

    -   一类是类型计算（通过不同的模板参数返回不同的类型），此类计算为构建类型系统提供了基础，也是泛型编程的基础
    -   一类是整型参数的算术运算, 此类计算提供了模板在实例化时候动态匹配模板的能力；实参通过计算后的结果作为新的实参去匹配特定模板（模板特化）

5.  模板递归

    模板递归是模板元编程的基础，也是C++11变参模板的基础

## 模版典型的应用场景有哪些

1.  C++ Library： 可以实现通用的容器（Containers）和算法（Algorithms），比如STL，Boost等,使用模板技术实现的迭代器（Iterators）和仿函数（Functors）可以很好让容器和算法可以自由搭配和更好的配合;

2.  C++ type traits：通过模板技术，C++ type traits实现了一套操作类型特性的系统，C++是静态类型语言，所以需要再编译时候对类型进行检查，这个时候type traits可以提供更多类型信息给编译器，能让程序可以做出更多选择和优化。 C++创始人对traits的理解：

    "Think of a trait as a small object whose main purpose is to carry information used by another object or algorithm to determine "policy" or "implementation details". - Bjarne Stroustrup

3.  Template metaprogramming-TMP：随着模板技术的发展，模板元编程逐渐被人们发掘出来，metaprogramming本意是进行源代码生成的编程（代码生成器），同时也是对编程本身的一种更高级的抽象，好比我们元认知这些概念，就是对学习本身更高级的抽象。 TMP通过模板实现一套“新的语言”（条件，递归，初始化，变量等），由于模板是图灵完备，理论上可以实现任何可计算编程，把本来在运行期实现部分功能可以移到编译期实现，节省运行时开销，比如进行循环展开，量纲分析等

    ![img](https://gitee.com/zyuegege/images/raw/master/imgs/1389958-20180501140243818-1007393312.png)

4.  Policy-Based Class Design

    C++ Policy class design 首见于 Andrei Alexandrescu 出版的 《Modern C++ Design》 一书以及他在C/C++ Users Journal杂志专栏 Generic<Programming>，参考wiki。通过把不同策略设计成独立的类，然后通过模板参数对主类进行配置,通常policy-base class design采用继承方式去实现，这要求每个策略在设计的时候要相互独立正交。STL还结合CRTP （Curiously recurring template pattern）等模板技术，实现类似动态多态（虚函数）的静态多态，减少运行开销

5.  Generic Programming

    由于模板这种对类型强有力的抽象能力，能让容器和算法更加通用，这一系列的编程手法，慢慢引申出一种新的编程范式：泛型编程。 泛型编程是对类型的抽象接口进行编程，STL库就是泛型编程经典范例

## 对模版的展望

1.  模版带来的缺点是什么？

    模板的代码和通常的代码比起来，可读性很差，所以很难维护，对人员要求非常高，开发和调试比较麻烦。 对模板代码，实际上很难覆盖所有的测试，所以为了保证代码的健壮性，需要大量高质量的测试，各个平台（编译器）支持力度不一样（模板递归深度，模板特性），可移植性不能完全保证。模板多个实例很有可能会隐式地增加二进制文件的大小等，所以模板在某些情况下有一定代价；

2.  基于模板的设计模式

    随着C++模板技术的发展，以及大量的经验总结，逐渐形成了一些基于模板的经典设计，比如STL里面的特性（traits），策略（policy），标签（tag）等技法；Andrei Alexandrescu 提出的Policy-Based Class Design；以及Jim Coplien的curiously recurring template pattern (CRTP)，以及衍生Mixin技法；或许未来，基于模板可以衍生更多的设计模式。

3.  模板的未来

    C++标准准备引进Concepts（Concepts: The Future of Generic Programming by Bjarne Stroustrup）解决模板调试问题，对模板接口进行约束；随着模板衍生出来的泛型编程，模板元编程，模板函数式编程等理念的发展，将来也许会发展出更抽象，更通用编程理念。模板本身是图灵完备的，所以可以结合C++其他特性：编译期常量和常量表达式，编译期计算，继承，友元friend等开阔出更多优雅的设计，比如元容器，类型擦除，自省和反射等，将来会出现更多设计

以上引用[C++Template 模版的本质](https://blog.csdn.net/lianhunqianr1/article/details/79966911)

---

# 模板的用法

## 模板的概念

1.  所谓模板是一种使用**无类型参数**来产生一系列**函数**或**类**的机制
2.  若一个程序的功能是对某种特定的数据类型进行处理，则可以将所处理的**数据类型说明为参数**，以便在其他数据类型的情况下使用，这就是模板的由来
3.  模板是以一种**完全通用的方法**来设计函数或类而不必预先说明将被使用的每个对象的类型
4.  通过模板可以产生类或函数的集合，使它们操作不同的数据类型，从而**避免需要为每一种数据类型产生一个单独的类或函数**

## 模板的分类

1.  模板分为函数模板（模子）和类模板（模子），允许用户分别用它们构造（套印）出（模板）函数和（模板）类

2.  下图显示了模板（函数模板和类模板），模板函数，模板类和对象之间的关系

    ![img](https://gitee.com/zyuegege/images/raw/master/imgs/1500965-d3148b2602eeaeb2.png)

### 函数模板

格式：

```c++
/**
 * ------------------------------
 * template<模板参数列表>
 * 返回值类型 函数名(模板函数形参列表) {
 *    // 函数体
 * }
 * ------------------------------
 * 模板形参表格式如下:
 * class/typename/类型修饰 参数名
 * class 与typename 是没有却别的，class 出现的比较早，现在一般使用typename
 */
template<typename T>
T const & max(T const &a, T const &b) {
    return a < b ? b : a;
}
```

其中T为类型参数，它可用基本类型或用户自定义的类型。模板函数的使用，只需要传入对应的值即可

```c++
std::cout << "max(7, 41):"<< ::max(7, 42) <<std::endl;
// 显示指定类型
std::cout << "max(7, 42):"<< ::max<int>(7, 42) <<std::endl;
std::cout << "max(7.0, 42.0):"<< ::max(7.0,42.0) <<std::endl;
std::cout << "max<int>(7.0, 42.0):"<< ::max<int>(7.0,42.0) <<std::endl;
std::cout << "max<double>(7.0, 42.0):"<< ::max<double>(7.0,42.0) <<std::endl
std::string s1 = "aaaa";
std::string s2 = "aaaabb";
std::cout << "max(s1,s s2):"<< ::max(s1, s2) <<std::endl;

// 个参数类型不相同时候会报错，例如
// max(7.0, 42) 
// candidate template ignored: deduced conflicting types for
//     parameter 'T' ('double' vs. 'int')
// inline T const& max(T const & a, T const & b)
```

通常而言，并不是把模板编译成一个可以处理任何类型的单一实体；而是对于实例化模板参数的每种类型，都从模板产生一个不同的实体。这种用具体类型代替模板参数的过程叫做**实例化**，它产生了一个模板的实例。于是，模板被编译了两次，分别发生在：

-   实例化之前，先检查模板代码本身，查看语法是否正确；在这里会发现错误的语法，如遗漏分号等
-   在实例化期间，检查模板代码，查看是否所有的调用都有效。在这里会发现无效的调用，如该实例化类型不支持某些函数调用(该类型没有提供模板所需要使用到的操作)等

#### 函数模板重载

```c++
template <typename T>
inline T const& max(T const & a, T const & b) {
    std::cout << "all" << std::endl;
    return a < b ? b : a;
}

inline int const& max(int const & a, int const & b){
    std::cout << "int" << std::endl;
    return a < b ? b : a;
}

int main() {
    int a = 7;
    int b = 42;
    int maxInt = ::max(a,b);
    std::cout << maxInt << std::endl; // 调用非模板
    std::cout << max<>(a,b) << std::endl; // 调用模板
    std::cout << max<int>(a,b) << std::endl; // 调用模板
    std::cout << max(1.0,2.0) << std::endl; // 调用非模板
    std::cout << max("aaa", "bbb") << std::endl; // 调用模板
    std::cout << max("42", 1) << std::endl; // 调用非模板
}
```

1.  对于非模板函数和同名的函数模板，如果其他条件都是相同的话，那么在调用的时候，重载解析过程通常会优**先调用非模板函数**，而不会从该模板产生出一个实例。然而，如果模板可以产生一个具有更好匹配的函数，那么将选择模板
2.  可以**显式地指定一个空的模板实参列表**，这个语法好像是告诉编译器：只有模板才能匹配这个调用（即便非模板函数更符合匹配条件也不会被调用到），而且所有的模板参数都应该根据调用实参演绎出来
3.  因为**模板是不允许自动类型转化**的；但**普通函数可以进行自动类型转换**，所以当一个匹配既没有非模板函数，也没有函数模板可以匹配到的时候，会尝试通过自动类型转换调用到非模板函数（前提是可以转换为非模板函数的参数类型）

### 类模板

格式：

关键字typename（或class）后面的T是类型参数，在实例化类定义中,欲采用通用数据类型的数据成员，成员函数的参数或返回值，前面需要加上T。类模板的内部可以想其他类一样，声明成员变量和成员函数。在成员函数的实现中必须要限定这个模板类，成员函数实现逻辑就是函数模板

```c++
/**
 * template<typename T>
 * class 类名 {
 *   //…
* };
 */

// 栈的类模板
#include <vector>
#include <stdexcept>

template <typename T>
class Stack {
    private:
        std::vector<T> elems;
    public:
        void push(T const & e); // 入栈
        void pop(); // 出栈
        T top() const; // 返回栈顶元素
        bool empty() const {    // 是否空
            return elems.empty();
        }
};

// 成员函数实现
// 入栈
template <typename T>
void Stack<T>::push(T const& e) {
   elems.push_back(e);
}
// 出栈
template <typename T>
void Stack<T>::pop() {
   if (elems.empty()) {
       throw std::out_of_range("out of range");
   }
   T e = elems.back();
   elems.pop_back(); // 删除最后一个元素
   return e;
}
// 返回栈顶原始
template <typename T>
T Stack<T>::top() const {
   if (elems.empty()) {
       throw std::out_of_range("out of range");
   }
   return elems.back(); // 返回最后一个元素的拷贝
}
```

类模板不代表一个具体的、实际的类,而代表一类类。实际上,类模板的使用就是将类模板实例化成一个具体的类，它的格式为: `类名<实际的类型>对象名`。例如,使用上面的类模板,创建模板参数为`char`、`int`型的对象,语句如下:

```c++
Stack<char> charStack ;
Stack<int> intStack;
intStack.push(1);
intStack.push(2);

std::cout << intStack.top() << std::endl;
```

1.  只有那些被调用的成员函数，才会产生这些函数的实例化代码。对于类模板，成员函数只有在被使用的时候才会被实例化。显然，这样可以节省空间和时间
2.  另一个好处是，对于那些“未能提供所有成员函数中所有操作的”类型，你也可以使用该类型来实例化类模板，只要对那些“未能提供某些操作的”成员函数，模板内部不使用就可以
3.  如果类模板中含有静态成员，那么用来实例化的每种类型，都会实例化这些静态成员。切记，要作为模板参数类型，唯一的要求就是：该类型必须提供被调用的所有操作

#### 类模板的特化

特化 和 重载类似，重载是函数名相同，形参不同，特化就是类名相同，类的具体类型不同；为了特化一个类模板，你必须在起始处声明一个`template<>`，接下来声明用来特化类模板的类型。这个类型被用作模板实参，且必须在类名的后面直接指定

```c++
template<>
class Stack<std::string> {
    // ...
};
```

进行类模板的特化时，每个成员函数都必须重新定义为普通函数，原来模板函数中的每个T也相应地被进行特化的类型取代。如:

```c++
void Stack<std::string>::push(std::string const& elem) {
    elems.push_back(elem);
}
```

#### 局部特化

上面的特化，类模板被具体类型代替、所有的成员函数被重新定义，这个叫做全特化；有时候要求模板参数仍由用户控制，这个叫做偏特化或者局部特化；
类模板

```c++
template <typename T1, typename T2>
class Myclass {
};
```

如下几种特化:

```c++
// 两个模板参数具有相同的类型
template <typename T>
class Myclass<T, T>  {
};

// 第2个模板参数的类型是int
template <typename T>
class Myclass<T, int> {
};

// 两个模板参数都是指针类型
template <typename T1, typename T2>
class Myclass<T1*, T2*>   { // 也可以使引用类型T&，常引用等
};
```

创建对象:

```c++
Myclass <int, float> m1;  // 使用 Myclass<T1, T2>
Myclass <float, float> m2; // 使用 Myclass<T, T>
Myclass <float, int> m3; // 使用 Myclass<T, int> 
Myclass <int*, float*> m4; // 使用 Myclass<T1*, T2*>
// Myclass<int, int>(); // Myclass<T, T>与Myclass<T, int>产生冲突歧义
```

如果创建对象时候，出现一个对象对应两个模板类，就会报错， 而对于函数模板，却只有全特化，不能偏特化：

以上引用[c++ 模板](https://www.jianshu.com/p/8156b4aae3bd)

#### 类模板的别名

1.  类模板定义了一个了类类型，与其他任何类类型一样，可以定义`typedef`别名来引用实例化的类：

    ```c++
    typedef Myclass<int, float> TypedefName;
    ```

2.  还可以使用`using`为类模板定义一个别名：

    ```c++
    template<typename T> 
    using twin = pair<T, T>;
    
    twin<string> authors; // authors 是一个pair<string, string>的类型
    ```

    当我们定义一个模板类的别名是，可以固定一个或多个模板参数：

    ```c++
    template<typename T>
    using partNo = pair<T, unsigned>;
    
    partNo<string> books; // books 是一个pair<string, unsigned>
    partNo<Vechicle> cars; // cars 是一个pair<Vechicle, unsigned>
    partNo<Student> kids; // kids 是一个pair<Student, unsigned>
    ```

#### 类模板的成员模板

对于类模板，我们也可以为其定义成员模板，在此情况下，类和成员各自有自己的独立的模板参数。例如，

```c++
template<typename T> class Blob {
    template<typename It> 
    Blob(It b, It e);
    // ...
};
```

此类构造函数有自己的类型模板参数It，作为其两个参数的类型。与类模板的普通成员不同，成员模板是函数模板。当我们在类模板外定义一个成员模板时，必须同时为类模板和成员模板提供模板参数列表。类模板参数在前，后面跟着成员自己的模板参数列表：

```c++
template<typename T> // 类的类型参数
template<typename It> // 构造函数的类型参数
Blob<T>::Blob(It b, It e):/*参数初始化列表*/{
    // ...
}
```

以上引用[C++ primer]()

### 模板的参数

模板的参数可以分为三种，类型参数、非类型参数、模板模板参数。

#### 模板的类型参数

我们可以将类型参数看说类型说明符，就像内置类型或类类型说明符一样使用。特别是，类型参数可以用来指定返回类型或者函数的参数类型，以及在函数体与类模板作用域内用于变量声明或类型转换。类型参数形参前面必须加关键值`class`或`typename`，为了和类声明区别开来，建议使用`typename`.

#### 模板的非类型参数

一个非类型参数表示一个值，而非一个类型。通过一个特定的类型名字而非关键字`class`和`typename`来指定非类型参数。无类型模板参数不能是对象，甚至不能是`double`或者`float`。无类型参数仅限于`int`、`enmu`、指针和左值引用，常量表达式。而对于指针或引用的非类型参数的实参必须绑定到具有静态生存期的对象

#### 模板模板参数

就是将一个模板作为另一个模板的参数，例如：

```c++
Grid<int,vector<int> > myIntGrid;
```

注意其中`int`出现了两次，必须指定`Grid`和`vector`的元素类型都是`int`。

```c++
Grid<int,vector> myIntGrid;
```

因为`vector`本身就是一个模板，而不是一个类型，所以这就是一个模板模板参数。指定模板模板参数有点像在常规的函数中指定函数指针参数。函数指针类型包括返回类型和函数的参数类型。首先要知道作为参数的模板的原型,比如`vector`：

```c++
template<typename E,typename Allocator=allocator<E> >
class vector
{...};
```

然后就可以定义：

```c++
template<typename T,template<typename E,typename Allocator=allocator<E> >class Container=vector>
class Grid {
    public:
    //Omitted for brevity
    Container<T>* mCells;
};
```

模板模板参数的一般语法：

```c++
template<other params,...,template<TemplateTypeParams> class ParameterName, other params,...>
```

举例一个应用，`Grid`的一个构造函数：

```c++
template<typename T,template<typename E,typename Allocator=allocator<E> >class Container>
Grid<T,Container>::Grid(int inWidth,int inHeight):
mWidth(inWidth),mHeight(inHeight) {
    mCells=new Container<T> [mWidth]; //注意此处Container<T>说明，实际上还是说明 Grid<int,vector<int> >
    for(int i=0;i<mWidth;++i) {
    	mCells[i].resize(mHeight);
    }
}
```

使用的时候，与一般的没有什么区别：

```c++
Grid<int,vector> myGrid;
myGrid.getElement(2,3);
```

注意：不要拘泥于它的语法实现，只要记住可以使用模板作为模板的一个参数。

以上引用[类模板三种类模板参数](https://www.cnblogs.com/lsgxeva/p/7689995.html)

#### 模板的可变参数

是C++11新增的最强大的特性之一，它对参数进行了高度泛化，它能表示0到任意个数、任意类型的参数。可变数目的参数被称为**参数包（parameter packet）**。存在两种参数包：**模板参数包（template parameter packet）**表示零个或多个模板参数、**函数参数包（function parameter packet）**表零个或多个函数参数。

在c++11新特性中用省略号来表示模板参数或函数参数表示一个包。例如`class...`、`typename...`来指出接下来的参数表示零个或多个类型的列表；一个类型名后面跟一个省略号表示零个或多个给定类型的参数的列表。在函数参数列表中，如果一个参数的类型是一个模板参数包，则此参数也是一个函数参数包。例如：

```c++
// Args 是一个模板参数包，rest是一个函数参数包
// Args 表示零个或多个模板类型参数
// reset 表示零个或多个函数参数
template<typename T, typename... Args>
void foo(const T &t, const Args & ... rest);
```

可以利用`sizeof...`运算符来计算一个参数包中函数参数的数目。

扩展（expand）一个包就将它分解为构成的元素，对于每个元素应用模式，获得扩展后的列表。通过在模式的右边放一个`...`来触发扩展。例如

```c++
template <typename T, typename ... Args>
ostream & print(ostream & os, const T &t, const Args & ... rest) { // 扩展Args
    os << t << ", ";
    return print(os, rest...);  // 扩展rest
}
```

考虑如下例子：

```c++
template <typename... Args>
ostream & errorMsg(ostream & os, const Args & ... rest) {
    // print(os, debug_rep(a1), debug_rep(a2), ..., debug_rep(an))
    return print(os, debug_rep(rest)...);
}
```

扩展中的模式会独立地应用与包中每一个元素。

以上引用[C++ Primer]()

---

# 元编程

利用c++的模板方法进行元编程，尤其是在c++11/14/17标准不断更新后。元编程作为一种新兴的编程方式，受到了越来越多的广泛关注。结合已有文献和个人实践，对有关 C++ 元编程进行了系统的分析。首先介绍了 C++ 元编程中的相关概念和背景，然后利用科学的方法分析了元编程的 **演算规则**、**基本应用** 和实践过程中的 **主要难点**，最后提出了对 C++ 元编程发展的 **展望**。

太难，没看懂，请移步到[浅谈 C++ 元编程](https://blog.csdn.net/Tencent_TEG/article/details/102577552)、[[C++11模版元编程](https://www.cnblogs.com/qicosmos/p/4480460.html)]、[C++模板元编程（C++ template metaprogramming）](https://www.cnblogs.com/liangliangh/p/4219879.html)

---

# type_traits

我们知道Traits是C++语言的一种高级特性。STL首先利用Traits技术对迭代器的特性做出规范, 制定出`iterator_traits`。后来SGI STL把它应用在迭代器以外的地方, 就有了`type_traits`的叫法。

我的理解是type_traits是算法用来获取对象的一些重要信息，比如说是不是`class`，是不是`function`，有没有`const`, `signed`, `volatile`这些修饰符，有没有无用的构造函数之类的技术，根据 对象的这些信息就可以实现相关的操作。关于`type_traits`的详细资料可参见[链接](http://www.cplusplus.com/reference/type_traits/?kw=type_traits)

有人可能会奇怪为什么要用`traits`或`type_traits`这些奇奇怪怪，绕来绕去的东西。用虚函数实现多态不就可以运行的时候实现不同的行为了吗？我的理解是，`traits`是编译时候就能决定的，虚函数多态是运行时候决定的，还得去查虚函数表，这样就比较慢。说白了还是为了效率的缘故 。Traits和Type Traits实现的基础是**函数模板+偏特化**，如下图所示：

![](https://gitee.com/zyuegege/images/raw/master/imgs/20170329150034468.jpeg)

注意这是C++ G2.9的代码。Type Traits首先定义了`__true_type`和`__false_type`这两个structure。然后给出了泛化和针对`int`和`double`的两个特化版本。可以看出，因为`int`和`double`都是Plain Old Data(POD)类型，所以它们的`default_constructor`, `copy_constructor`, `assignment_operator`, `destructor` 这些东西都不重要，其实根本就不需要，所以它们的`has_trivial_xxxx`都被定义为`__true_type`。而泛化版本的这些缺省都是 `_false_type`。这样，当算法询问一个 POD类型的对象有没有`copy_contructor`的时候， 就知道答案为否。
我们看看对于一个比较简单的`class`(不含指针)，它的这些`has_trival_xxxx`会返回什么呢？我们可以测试一下这个Foo类：

```c++
class Foo {
  private:
     int d1, d2;
};

// 测试结果：
// __has_trivial_assign 1
// __has_trivial_copy 1
// __has_trivial_constructor 1
// __has_trivial_destructor 1
```

这个符合预期，因为`class Foo`里面没有指针，所以上面4个函数都不重要，C++编译器给它们提供的缺省函数就够了。我们再测试一下`list`，返回结果为:

```c++
// __has_trivial_assign 0
// __has_trivial_copy 0
// __has_trivial_constructor 0
// __has_trivial_destructor 0
```

这个也符合预期， 因为list里面有指针嘛。看来这个`type_traits`很智能啊。它是怎么实现的呢?对于POD类型，我们举一个简单的`type_traits` `is_integral`，它用来看一个类型是不是整型，如果该类型是`int`/`unsigned int`/`long`/`unsigned long`/`long long`/`unsigned long long`/`bool`/`char`/`signed char`/`unsigned char` 之一，就返回`true`，否则就是`false`。
![](https://gitee.com/zyuegege/images/raw/master/imgs/20170329160004501.jpeg)

我们可以看出它其实就是在玩类模板和特化而已。如果深入到类的话，应该就没这么简单了。应该是编译器里面加了很多代码，比如说看一个class的话里面有没有指针，从而做出相应判断。

以上引用[C++ Type Traits的学习 (Boolan学习笔记第九周)](https://blog.csdn.net/roufoo/article/details/63685043)

# type_traits 源码分析

去年做项目的时候接触到`type_traits`，当时简单的阅读了一下源码，有一个粗浅的认识。适逢秋招之季，在复习c/c++基础的时候，又遇到了，于是决定再读一次源码，并做一下简单的记录。网上有许多关于`type_traits`讲解的很好，从原理，实现方式，与使用方法都做了详细的讲解例如[type_traits](#type_traits)、[C++范型编程 －－ 头文件](https://www.cnblogs.com/64open/p/5205558.html)、[C++type_traits](https://www.jianshu.com/p/b09fb9ae56dd)、[c++11——type_traits 类型萃取](https://www.cnblogs.com/gtarcoder/p/4807670.html)

源码路径：`/usr/include/c++/8/type_traits`

```c++
/// integral_constant
template<typename _Tp, _Tp __v>
    struct integral_constant
    {
      static constexpr _Tp                  value = __v;
      typedef _Tp                           value_type;
      typedef integral_constant<_Tp, __v>   type;
      constexpr operator value_type() const noexcept { return value; }
#if __cplusplus > 201103L

#define __cpp_lib_integral_constant_callable 201304

      constexpr value_type operator()() const noexcept { return value; }
#endif
};

template<typename _Tp, _Tp __v>
	constexpr _Tp integral_constant<_Tp, __v>::value;
```

`integral_constant`是`type_traits`里面最核心的一个，也是最基础的一个模板类型，表示一个整型常量，是能够在编译阶段就决定的。该模板有两个模板参数，一个是类型`_TP`，与一个`_TP`类型的值`__v`(常量值)，并且该常量值存储在静态常量`value`中，并将`_TP`重新命名为`value_type`,然后有一个`value_type()`的常量函数，用于获取该常量的值。

```c++
  /// The type used as a compile-time boolean with true value.
  typedef integral_constant<bool, true>     true_type;

  /// The type used as a compile-time boolean with false value.
  typedef integral_constant<bool, false>    false_type;

  template<bool __v>
    using __bool_constant = integral_constant<bool, __v>;

#if __cplusplus > 201402L
# define __cpp_lib_bool_constant 201505
  template<bool __v>
    using bool_constant = integral_constant<bool, __v>;
#endif
```

然后利用全特化特化`true_type`、`false_type`，与偏特化的`__bool_constant`(c++14以后提供`bool_constant`)。

接下来是元编程的一些辅助类型

```c++
// 声明一个conditional的条件类，其定义在后面
// 意思是，当第一个参数是true的时候，conditional::type为第二个模板参数
// 否则第二个蚕食是false的时候，conditional::type为第三个模板参数
template<bool, typename, typename>
    struct conditional;

// *********************conditional ******************
  // Primary template.
  /// Define a member typedef @c type to one of two argument types.
  template<bool _Cond, typename _Iftrue, typename _Iffalse>
    struct conditional
    { typedef _Iftrue type; };

  // Partial specialization for false.
  template<typename _Iftrue, typename _Iffalse>
    struct conditional<false, _Iftrue, _Iffalse>
    { typedef _Iffalse type; };
// *********************conditional ******************

// 声明一个模板类__or__接受零个或多个integral_constant或其继承类型，
// 该模板的作用是对多个integral_constant::value执行条件的或操作，并且__or_继承其相或的integral_const类型
  template<typename...>
    struct __or_;

// 定义当__or__有零个参数的时候，继承为false_type
  template<>
    struct __or_<>
    : public false_type
    { };

// 定义当__or_只有一个参数的时候，继承其模板参数类型
  template<typename _B1>
    struct __or_<_B1>
    : public _B1
    { };

// 定义当__or_有两个参数的时候，如果_B1::value为true则继承_B1
  template<typename _B1, typename _B2>
    struct __or_<_B1, _B2>
    : public conditional<_B1::value, _B1, _B2>::type
    { };

// 最后针对三个以上的参数，利用递归的方式展开完成
  template<typename _B1, typename _B2, typename _B3, typename... _Bn>
    struct __or_<_B1, _B2, _B3, _Bn...>
    : public conditional<_B1::value, _B1, __or_<_B2, _B3, _Bn...>>::type
    { };

template<typename...>
    struct __and_;

  template<>
    struct __and_<>
    : public true_type
    { };

  template<typename _B1>
    struct __and_<_B1>
    : public _B1
    { };

  template<typename _B1, typename _B2>
    struct __and_<_B1, _B2>
    : public conditional<_B1::value, _B2, _B1>::type
    { };

  template<typename _B1, typename _B2, typename _B3, typename... _Bn>
    struct __and_<_B1, _B2, _B3, _Bn...>
    : public conditional<_B1::value, __and_<_B2, _B3, _Bn...>, _B1>::type
    { };

  template<typename _Pp>
    struct __not_
    : public __bool_constant<!bool(_Pp::value)>
    { };
```



