---
layout: post
title: 测试
categories: 测试
date: 2020-06-03 10:17:09 +0800
---

[TOC]

---
# ceshi 
你好，我是张越，这是一个测试的post。$h_hh_h$

## sssss

_HHHHHHHH_

$$sss_sss$$


$$
\alpha_t = \left\{\begin{array}{l}\alpha \;\;\;\;\;\;\;\;\;\;\;\mbox{ if } y = 1\\ 1-\alpha \;\;\;\;\;\;\mbox{otherwise.}\end{array}\right.
$$


```python
#include
def add(x:int, y:int)->int:
    return x + y
```



$$\begin{equation}f(x) = x^2\end{equation}$$

```c++
#include <stdio.h>
int main(int argc, char **argv)
{
    return 0;
}
```

# 关键数据结构

### ngx_module_s

Nginx中所有模块都会对应一个`ngx_module_s`的数据结构对象，一般是以全局变量的形式定义所有的模块，例如在每一个模块中的源程序文件中都会由一个这样结构的全局变量。

```c++
struct ngx_module_s {
    ngx_uint_t            ctx_index;
    ngx_uint_t            index;

    char                 *name;

    ngx_uint_t            spare0;
    ngx_uint_t            spare1;

    ngx_uint_t            version;
    const char           *signature;

    void                 *ctx;
    ngx_command_t        *commands; // 配置文件中的命令格式与对应的解析函数 
    ngx_uint_t            type;

    ngx_int_t           (*init_master)(ngx_log_t *log); // master 进程初始化函数

    ngx_int_t           (*init_module)(ngx_cycle_t *cycle); // 模块初始化函数

    ngx_int_t           (*init_process)(ngx_cycle_t *cycle); // 在子进程中初始化函数
    ngx_int_t           (*init_thread)(ngx_cycle_t *cycle); // 线程中的初始化函数
    void                (*exit_thread)(ngx_cycle_t *cycle);	// 退出线程函数
    void                (*exit_process)(ngx_cycle_t *cycle);	// 退出进程处理函数

    void                (*exit_master)(ngx_cycle_t *cycle);	// 退出master函数

    uintptr_t             spare_hook0;
    uintptr_t             spare_hook1;
    uintptr_t             spare_hook2;
    uintptr_t             spare_hook3;
    uintptr_t             spare_hook4;
    uintptr_t             spare_hook5;
    uintptr_t             spare_hook6;
    uintptr_t             spare_hook7;
};
```

### ngx_core_module_t

核心模块的类型

```c++
typedef struct {
    ngx_str_t             name;
    void               *(*create_conf)(ngx_cycle_t *cycle);
    char               *(*init_conf)(ngx_cycle_t *cycle, void *conf);
} ngx_core_module_t;
```

---

# 全局变量

### ngx_modules

用于存放所有模块的指针数组

```c++
extern ngx_module_t  *ngx_modules[];
```

### ngx_max_module

最大的模块数量

```c++
extern ngx_uint_t     ngx_max_module;
```

### ngx_module_names

所有模块的名字

```c++
extern char          *ngx_module_names[];
```

---

# 函数

### ngx_preinit_modules

在初始化的时候调用，为所有的模块初始化索引，并为其赋值对应的名字，以及对`ngx_modules_n`和`ngx_max_module`赋值。

```c++
/**
 * ngx_preinit_modules
 * 在初始化的时候调用，为所有的模块初始化索引，
 * 并为其赋值对应的名字，以及对ngx_modules_n和ngx_max_module赋值
 * 
 * 返回NGX_OK
 */
ngx_int_t ngx_preinit_modules(void);
```

### ngx_cycle_modules

为当前`cycle`创建一个列表并初始化为`ngx_modules`的值。

```c++
/**
 * ngx_cycle_modules
 * 为当前cycle创建一个列表并初始化为ngx_modules的值
 *
 * @cycle 当前cycle
 * 
 * 返回NGX_OK
 */
ngx_int_t ngx_cycle_modules(ngx_cycle_t *cycle);
```

### ngx_init_modules

初始化当前`cycle`的所有模块，调用`ngx_module_s`的`init_module`成员方法。

```c++
/**
 * ngx_init_modules
 * 初始化当前cycle的所有模块，调用ngx_module_s的init_module成员方法
 *
 * @cycle 当前cycle
 *
 * 初始化成功返回NGX_OK，出错返回NGX_ERROR
 */
ngx_int_t ngx_init_modules(ngx_cycle_t *cycle);
```

### ngx_count_modules

统计所有指定`type`的模块，并未其赋值索引`ctx_index`

```c++
/**
 * ngx_count_modules
 * 统计所有指定type的模块，并未其赋值索引ctx_index
 * @cycle 指定cycle
 * @type 指定要统计的type
 * 返回该type的模块数量
 */
ngx_int_t ngx_count_modules(ngx_cycle_t *cycle, ngx_uint_t type);
```

### ngx_add_module

根据配置信息添加模块

```c++
/**
 * ngx_add_module
 * 根据配置信息添加模块
 * @cf 配置信息
 * @file 模块文件
 * @module 要添加的模块
 * @order顺序
 * 成功返回NGX_OK，出错返回NGX_ERROR
 */
ngx_int_t ngx_add_module(ngx_conf_t *cf, ngx_str_t *file,
    ngx_module_t *module, char **order);
```

[=0% "0%"]