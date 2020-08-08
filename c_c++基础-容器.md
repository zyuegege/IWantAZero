---
title: C/C++基础合集：容器
layout: post
date: 2020-06-13 20:20:00 +0800
categories: 秋招
tags:
	- c/c++
	- 面经
	- 秋招
---

在c++中容器是STL中的重要内容，也是c++标准中重要的内容，所有的泛型通用算法其实都是基于容器与迭代器的，所以容器技术也成为c++的核心内容，也是本次准备秋招的重点部分。

容器库是类模板与算法的汇集，允许程序员简单地访问常见数据结构，例如队列、链表和栈。有三类容器——顺序容器、关联容器和无序关联容器——每种都被设计为支持不同组的操作。容器管理为其元素分配的存储空间，并提供直接或间接地通过迭代器（拥有类似指针属性的对象）访问它们的函数。大多数容器拥有至少几个常见的成员函数，并共享功能。特定应用的最佳容器不仅依赖于提供的功能，还依赖于对于不同工作量的效率。

[TOC]

# 顺序容器

## array

```c++
#include <array>
template<
    class T,
    std::size_t N
> struct array;
```



`std::array` 是封装固定大小数组的容器。此容器是一个聚合类型，其语义等同于保有一个 [C 风格数组](https://zh.cppreference.com/w/cpp/language/array) T[N] 作为其唯一非静态数据成员的结构体。不同于 C 风格数组，它不会自动退化成 `T*` 。它能作为聚合类型[聚合初始化](https://zh.cppreference.com/w/cpp/language/aggregate_initialization)，只要有至多 `N` 个能转换成 `T` 的初始化器：` std::array<int, 3> a = {1,2,3}; `。该结构体结合了 C 风格数组的性能、可访问性与容器的优点，比如可获取大小、支持赋值、随机访问迭代器等。

`std::array` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求，除了默认构造的 `array` 是非空的，以及进行交换的复杂度是线性，它满足[*连续容器* *(ContiguousContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ContiguousContainer)(C++17 起)的要求并部分满足[*序列容器* *(SequenceContainer)* ](https://zh.cppreference.com/w/cpp/named_req/SequenceContainer)的要求。

当其长度为零时 `array` （ `N == 0` ）有特殊情况。此时， array.begin() == array.end() ，并拥有某个唯一值。在零长 `array` 上调用 `front()` 或 `back()` 是未定义的。亦可将 `array` 当做拥有 `N` 个同类型元素的元组。

按照规则，指向 `array` 的迭代器在 `array` 的生存期间决不非法化。然而要注意，在 `swap` 时，迭代器将继续指向同一 `array` 的元素，并将改变元素的值。

## vector

```c++
#include <vector>

// std::vector 是封装动态数组的顺序容器。
template<
    class T,
    class Allocator = std::allocator<T>
> class vector;

// std::pmr::vector 是使用多态分配器的模板别名。
namespace pmr {
    template <class T>
    using vector = std::vector<T, std::pmr::polymorphic_allocator<T>>;
}
```

元素相继存储，这意味着不仅可通过迭代器，还能用指向元素的常规指针访问元素。这意味着指向 `vector` 元素的指针能传递给任何期待指向数组元素的指针的函数。

`vector` 的存储是自动管理的，按需扩张收缩。 `vector` 通常占用多于静态数组的空间，因为要分配更多内存以管理将来的增长。 `vector` 所用的方式不在每次插入元素时，而只在额外内存耗尽时重分配。分配的内存总量可用 `capacity()` 函数查询。额外内存可通过对 `shrink_to_fit()` 的调用返回给系统。 (C++11 起)

重分配通常是性能上有开销的操作。若元素数量已知，则 `reserve()` 函数可用于消除重分配。vector 上的常见操作复杂度（效率）如下：

-   随机访问——常数 *O(1)*
-   在末尾插入或移除元素——均摊常数 *O(1)*
-   插入或移除元素——与到 vector 结尾的距离成线性 *O(n)*

`std::vector` （对于 `bool` 以外的 `T` ）满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*序列容器* *(SequenceContainer)* ](https://zh.cppreference.com/w/cpp/named_req/SequenceContainer)、[*连续容器* *(ContiguousContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ContiguousContainer)(C++17 起)及[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

模板参数：

-   `T`:  元素的类型。
    -   `T` 必须满足[*可复制赋值* *(CopyAssignable)* ](https://zh.cppreference.com/w/cpp/named_req/CopyAssignable)和[*可复制构造* *(CopyConstructible)* ](https://zh.cppreference.com/w/cpp/named_req/CopyConstructible)的要求。
    -   加诸元素的要求依赖于容器上进行的实际操作。泛言之，要求元素类型是完整类型并满足[*可擦除* *(Erasable)* ](https://zh.cppreference.com/w/cpp/named_req/Erasable)的要求，但许多成员函数附带了更严格的要求。
    -   加诸元素的要求依赖于容器上进行的实际操作。泛言之，要求元素类型满足[*可擦除* *(Erasable)* ](https://zh.cppreference.com/w/cpp/named_req/Erasable)的要求，但许多成员函数附带了更严格的要求。若分配器满足[分配器完整性要求](https://zh.cppreference.com/w/cpp/named_req/Allocator#.E5.88.86.E9.85.8D.E5.99.A8.E5.AE.8C.E6.95.B4.E6.80.A7.E8.A6.81.E6.B1.82)，则容器（但非其成员）能以不完整元素类型实例化。
-    用于获取/释放内存及构造/析构内存中元素的分配器。类型必须满足[*分配器* *(Allocator)* ](https://zh.cppreference.com/w/cpp/named_req/Allocator)的要求。若 `Allocator::value_type` 与 `T` 不同则行为未定义。

标准库提供 `std::vector` 对类型 `bool` 的特化，它可能为空间效率优化。

1.  vector底层原理，resize和reserve方法原理?

    底层存储采用连续空间方式进行存储。

-   `resize`重新申请并改变当前`vector`对象的有效空间大小，并可设置默认值；如果sz小于当等于前容量大小，则不改变容量大小，如果sz大于当前容量大小，则重新分配大小，容量等于sz；
-   `reserve`:重新申请并改变当前`vector`对象的总空间（`_capacity`）大小，如果reserve的参数小于当等于当前size，则不会更改容量大小；

2.  C++ 的vector的底层数据结构是什么？为什么2倍扩容？

    当数组大小不够容纳新增元素时，开辟更大的内存空间，把旧空间上的数据复制过来，然后在新空间中继续增加。新的更大的内存空间，一般是当前空间的1.5倍或者2倍，这个1.5或者2被称为扩容因子，不同系统实现扩容因子也不同。**注意，扩容会导致迭代器失效。**

    扩容因子越大，需要分配的新内存空间越多，越耗时。空闲空间较多，内存利用率低。

    扩容因子越小，需要再分配的可能性就更高，多次扩容耗时。空闲空间较少，内存利用率高。

    因此，小规模数组，添加元素不频繁的，建议使用扩容因子更小的。当数据规模越大，插入更频繁，大扩容因子更适合。

## deque

`std::deque` （ double-ended queue ，双端队列）是有下标顺序容器，它允许在其首尾两段快速插入及删除。另外，在 `deque` 任一端插入或删除不会非法化指向其余元素的指针或引用。

与 `std::vector`相反， `deque` 的元素不是相接存储的：典型实现用单独分配的固定大小数组的序列，外加额外的登记，这表示下标访问必须进行二次指针解引用，与之相比 `vector` 的下标访问只进行一次。

`deque` 的存储按需自动扩展及收缩。扩张 `deque` 比扩张 `std::vector` 更优，因为它不涉及到复制既存元素到新内存位置。另一方面， `deque` 典型地拥有较大的最小内存开销；只保有一个元素的 `deque` 必须分配其整个内部数组（例如 64 位 libstdc++ 上为对象大小 8 倍； 64 位 libc++ 上为对象大小 16 倍或 4096 字节的较大者）。

```c++
#include <deque>
template<
    class T,
    class Allocator = std::allocator<T>
> class deque;

namespace pmr {
    template <class T>
    using deque = std::deque<T, std::pmr::polymorphic_allocator<T>>;
}
```

`deque` 上常见操作的复杂度（效率）如下：

-   随机访问——常数 *O(1)*
-   在结尾或起始插入或移除元素——常数 *O(1)*
-   插入或移除元素——线性 *O(n)*

`std::deque` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*序列容器* *(SequenceContainer)* ](https://zh.cppreference.com/w/cpp/named_req/SequenceContainer)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

双向队列的实现的核心是其迭代器的实现，下面是双向队列迭代器的实现形式，只有关键部分的代码。

![img](https://gitee.com/zyuegege/images/raw/master/imgs/IMG_20200616_101141.jpg)

```c++
#define _GLIBCXX_DEQUE_BUF_SIZE 512 

_GLIBCXX_CONSTEXPR inline size_t
  __deque_buf_size(size_t __size)
  { return (__size < _GLIBCXX_DEQUE_BUF_SIZE
	    ? size_t(_GLIBCXX_DEQUE_BUF_SIZE / __size) : size_t(1)); }


template<typename _Tp, typename _Ref, typename _Ptr>
    struct _Deque_iterator
    {
    private:
      template<typename _Up>
	using __ptr_to = typename pointer_traits<_Ptr>::template rebind<_Up>;
      template<typename _CvTp>
	using __iter = _Deque_iterator<_Tp, _CvTp&, __ptr_to<_CvTp>>;
    public:
      typedef __iter<_Tp>		iterator;
      typedef __iter<const _Tp>		const_iterator;
      typedef __ptr_to<_Tp>		_Elt_pointer;
      typedef __ptr_to<_Elt_pointer>	_Map_pointer;
 
      static size_t _S_buffer_size() _GLIBCXX_NOEXCEPT
      { return __deque_buf_size(sizeof(_Tp)); }       
        
      typedef std::random_access_iterator_tag	iterator_category;
      typedef _Tp				value_type;
      typedef _Ptr				pointer;
      typedef _Ref				reference;
      typedef size_t				size_type;
      typedef ptrdiff_t				difference_type;
      typedef _Deque_iterator			_Self;

      _Elt_pointer _M_cur;
      _Elt_pointer _M_first;
      _Elt_pointer _M_last;
      _Map_pointer _M_node;
        
        // 其他函数

      _Self&
      operator++() _GLIBCXX_NOEXCEPT
      {
	++_M_cur;
	if (_M_cur == _M_last)
	  {
	    _M_set_node(_M_node + 1);
	    _M_cur = _M_first;
	  }
	return *this;
      }
        
      _Self&
      operator--() _GLIBCXX_NOEXCEPT
      {
	if (_M_cur == _M_first)
	  {
	    _M_set_node(_M_node - 1);
	    _M_cur = _M_last;
	  }
	--_M_cur;
	return *this;
      }
        
      /**
       *  Prepares to traverse new_node.  Sets everything except
       *  _M_cur, which should therefore be set by the caller
       *  immediately afterwards, based on _M_first and _M_last.
       */
      void
      _M_set_node(_Map_pointer __new_node) _GLIBCXX_NOEXCEPT
      {
	_M_node = __new_node;
	_M_first = *__new_node;
	_M_last = _M_first + difference_type(_S_buffer_size());
      }
    }


      //This struct encapsulates the implementation of the std::deque
      //standard container and at the same time makes use of the EBO
      //for empty allocators.
      struct _Deque_impl
      : public _Tp_alloc_type
      {
	_Map_pointer _M_map;
	size_t _M_map_size;
	iterator _M_start;
	iterator _M_finish;
      };
        
  /**
   *  @brief Layout storage.
   *  @param  __num_elements  The count of T's for which to allocate space
   *                          at first.
   *  @return   Nothing.
   *
   *  The initial underlying memory layout is a bit complicated...
  */
  template<typename _Tp, typename _Alloc>
    void
    _Deque_base<_Tp, _Alloc>::
    _M_initialize_map(size_t __num_elements)
    {
      const size_t __num_nodes = (__num_elements/ __deque_buf_size(sizeof(_Tp))
				  + 1);

      this->_M_impl._M_map_size = std::max((size_t) _S_initial_map_size,
					   size_t(__num_nodes + 2));
      this->_M_impl._M_map = _M_allocate_map(this->_M_impl._M_map_size);

      // For "small" maps (needing less than _M_map_size nodes), allocation
      // starts in the middle elements and grows outwards.  So nstart may be
      // the beginning of _M_map, but for small maps it may be as far in as
      // _M_map+3.

      _Map_pointer __nstart = (this->_M_impl._M_map
			       + (this->_M_impl._M_map_size - __num_nodes) / 2);
      _Map_pointer __nfinish = __nstart + __num_nodes;

      __try
	{ _M_create_nodes(__nstart, __nfinish); }
      __catch(...)
	{
	  _M_deallocate_map(this->_M_impl._M_map, this->_M_impl._M_map_size);
	  this->_M_impl._M_map = _Map_pointer();
	  this->_M_impl._M_map_size = 0;
	  __throw_exception_again;
	}

      this->_M_impl._M_start._M_set_node(__nstart);
      this->_M_impl._M_finish._M_set_node(__nfinish - 1);
      this->_M_impl._M_start._M_cur = _M_impl._M_start._M_first;
      this->_M_impl._M_finish._M_cur = (this->_M_impl._M_finish._M_first
					+ __num_elements
					% __deque_buf_size(sizeof(_Tp)));
    }          

```



## forward_list

`std::forward_list` 是支持从容器中的任何位置快速插入和移除元素的容器。不支持快速随机访问。它实现为单链表，且实质上与其在 C 中实现相比无任何开销。与 `std::list`相比，此容器在不需要双向迭代时提供更有效地利用空间的存储。

在链表内或跨数个链表添加、移除和移动元素，不会非法化当前指代链表中其他元素的迭代器。然而，在从链表移除元素（通过 [erase_after](https://zh.cppreference.com/w/cpp/container/forward_list/erase_after) ）时，指代对应元素的迭代器或引用会被非法化。

`std::forward_list` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)（除了 `operator==` 的复杂度始终为线性和 `size` 函数）、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)和[*序列容器* *(SequenceContainer)* ](https://zh.cppreference.com/w/cpp/named_req/SequenceContainer)的要求。

```c++
#include <forward_list>
template<
    class T,
    class Allocator = std::allocator<T>
> class forward_list;

namespace pmr {
    template <class T>
    using forward_list = std::forward_list<T, std::pmr::polymorphic_allocator<T>>;
}
```



## list

`std::list` 是支持常数时间从容器任何位置插入和移除元素的容器。不支持快速随机访问。它通常实现为双向链表。与 `std::forward_list`相比，此容器提供双向迭代但在空间上效率稍低。

在 `list` 内或在数个 `list` 间添加、移除和移动元素不会非法化迭代器或引用。迭代器仅在对应元素被删除时非法化。

`std::list` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*序列容器* *(SequenceContainer)* ](https://zh.cppreference.com/w/cpp/named_req/SequenceContainer)及[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

```c++
#include <list>

template<
    class T,
    class Allocator = std::allocator<T>
> class list;

namespace pmr {
    template <class T>
    using list = std::list<T, std::pmr::polymorphic_allocator<T>>;
}
```



# 关联容器

## set

`std::set` 是关联容器，含有 `Key` 类型对象的已排序集。用比较函数 [*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)进行排序。搜索、移除和插入拥有对数复杂度。 `set` 通常以[红黑树](https://en.wikipedia.org/wiki/Red–black_tree)实现。

在每个标准库使用[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)概念的场所，用等价关系确定唯一性。不精确地说，若二个对象 `a` 与 `b` 相互间既不比较大于亦不比较小于： `!comp(a, b) && !comp(b, a)` ，则认为它们等价。

`std::set` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*关联容器* *(AssociativeContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AssociativeContainer)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

```c++
#include <set>

template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class set;

namespace pmr {
    template <class Key, class Compare = std::less<Key>>
    using set = std::set<Key, Compare, std::pmr::polymorphic_allocator<Key>>;
}
```



## map

`std::map` 是有序键值对容器，它的元素的键是唯一的。用比较函数 `Compare` 排序键。搜索、移除和插入操作拥有对数复杂度。 map 通常实现为[红黑树](https://en.wikipedia.org/wiki/Red–black_tree)。

在每个标准库使用[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)概念的位置，以等价关系检验唯一性。不精确而言，若二个对象 `a` 与 `b` 互相比较不小于对方 ： `!comp(a, b) && !comp(b, a)` ，则认为它们等价（非唯一）。

`std::map` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*关联容器* *(AssociativeContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AssociativeContainer)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

```c++
#include <map>

template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T> >
> class map;

namespace pmr {
    template <class Key, class T, class Compare = std::less<Key>>
    using map = std::map<Key, T, Compare,
                         std::pmr::polymorphic_allocator<std::pair<const Key,T>>>
}
```



## multiset

`std::multiset` 是含有 Key 类型对象有序集的容器。不同于 set ，它允许多个关键拥有等价的值。用关键比较函数 Compare 进行排序。搜索、插入和移除操作拥有对数复杂度。

在标准库使用[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)概念的每处，都用描述于[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)的等价关系确定等价性。不精确地说，若二个对象 `a` 和 `b` 互不比较小于对方： `!comp(a, b) && !comp(b, a)` ，则认为它们等价。

比较等价的元素顺序是插入顺序，而且不会更改。(C++11 起)

`std::multiset` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*关联容器* *(AssociativeContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AssociativeContainer)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

```c++
#include <set>

template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class multiset;

namespace pmr {
    template <class Key, class Compare = std::less<Key>>
    using multiset = std::multiset<Key, Compare,
                                   std::pmr::polymorphic_allocator<Key>>;
}
```



## mulitimap

multimap 是关联容器，含有键值对的已排序列表，同时容许多个元素拥有同一键。按照应用到键的比较函数 `Compare` 排序。搜索、插入和移除操作拥有对数复杂度。

拥有等价键的键值对的顺序就是插入顺序，且不会更改。(C++11 起)

凡在标准库使用[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)概念出，都用描述于[*比较* *(Compare)* ](https://zh.cppreference.com/w/cpp/named_req/Compare)上的等价关系确定等价性。不精确地说，若二个对象 `a` 和 `b` 互不小于对方： `!comp(a, b) && !comp(b, a)` ，则认为它们等价。

`std::multimap` 满足[*容器* *(Container)* ](https://zh.cppreference.com/w/cpp/named_req/Container)、[*具分配器容器* *(AllocatorAwareContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AllocatorAwareContainer)、[*关联容器* *(AssociativeContainer)* ](https://zh.cppreference.com/w/cpp/named_req/AssociativeContainer)和[*可逆容器* *(ReversibleContainer)* ](https://zh.cppreference.com/w/cpp/named_req/ReversibleContainer)的要求。

```c++
#include <map>

template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T> >
> class multimap;

namespace pmr {
    template <class Key, class T, class Compare = std::less<Key>>
    using multimap = std::multimap<Key, T, Compare,
                                  std::pmr::polymorphic_allocator<std::pair<const Key,T>>>;
}
```



# 无序关联容器

## unordered_set

## unordered_map

## unordered_multiset

## unordered_multimap

# 容器适配器

## stack

## queue

## priority_queue

# span

# 迭代器

遗留输入迭代器 (LegacyInputIterator) 、遗留输出迭代器 (LegacyOutputIterator) 、遗留向前迭代器 (LegacyForwardIterator) 、遗留双向迭代器 (LegacyBidirectionalIterator) 、遗留随机访问迭代器 (LegacyRandomAccessIterator) ，及遗留连续迭代器 (LegacyContiguousIterator) (C++17 起)。

迭代器的分类的依据并不是迭代器的类型，而是迭代器所支持的操作。换句话说，某个类型只要支持相应的操作，就可以作为迭代器使用。例如，完整对象类型指针支持所有[*遗留随机访问迭代器* *(LegacyRandomAccessIterator)* ](https://zh.cppreference.com/w/cpp/named_req/RandomAccessIterator)要求的操作，于是任何需要[*遗留随机访问迭代器* *(LegacyRandomAccessIterator)* ](https://zh.cppreference.com/w/cpp/named_req/RandomAccessIterator)的地方都可以使用指针。

迭代器的所有类别（除了[*遗留输出迭代器* *(LegacyOutputIterator)* ](https://zh.cppreference.com/w/cpp/named_req/OutputIterator)和[*遗留连续迭代器* *(LegacyContiguousIterator)* ](https://zh.cppreference.com/w/cpp/named_req/ContiguousIterator)）能组织到层级中，其中更强力的迭代器类别（如[*遗留随机访问迭代器* *(LegacyRandomAccessIterator)* ](https://zh.cppreference.com/w/cpp/named_req/RandomAccessIterator)）支持较不强力的类别（例如[*遗留输入迭代器* *(LegacyInputIterator)* ](https://zh.cppreference.com/w/cpp/named_req/InputIterator)）的所有操作。若迭代器落入这些类别之一且亦满足[*遗留输出迭代器* *(LegacyOutputIterator)* ](https://zh.cppreference.com/w/cpp/named_req/OutputIterator)的要求，则称之为*可变* 迭代器并且支持输入*还有*输出。称非可变迭代器为*常*迭代器。

