---
layout: post
title: LeetCode刷题笔记
date: 2020-06-10 21:51:00 +0800
categories: 秋招
tags:
	- c/c++
	- 秋招
	- leetcode
	- 算法
---

[TOC]

# 10. 正则表达式匹配

题目描述 [链接](https://leetcode-cn.com/problems/regular-expression-matching/) ：

给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

所谓匹配，是要涵盖**整个**字符串 `s`的，而不是部分字符串。

**说明:**

- `s` 可能为空，且只包含从`a-z`的小写字母。
- `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。

## 题解1：递归

每次只判断其首个字符是否匹配，如果首个字符匹配，则继续匹配；如果某个字符后面跟的是`*`则考虑其匹配零个和大于零个首字符两种情况。

```c++
// 回溯法
bool isMatch(const string s, const string p) {
    if (p.empty())
        return s.empty();

    bool first_matched = (!s.empty()) && (s[0] == p[0] || p[0] == '.');

    if (p.length() >= 2 && p[1] == '*') {
        return isMatch(s, p.substr(2)) || (first_matched && isMatch(s.substr(1), p));
    }
    
    return first_matched && isMatch(s.substr(1), p.substr(1));
}
```

## 题解2：动态规划

因为题目拥有 最优子结构 ，一个自然的想法是将中间结果保存起来。我们通过用$dp(i,j)$ 表示$text[i:]$ 和$pattern[j:] $是否能匹配。我们可以用更短的字符串匹配问题来表示原本的问题。

我们用 [题解1](#题解1：递归) 中同样的回溯方法，除此之外，因为函数 `match(text[i:], pattern[j:])` 只会被调用一次，我们用$dp(i, j)$ 来应对剩余相同参数的函数调用，这帮助我们节省了字符串建立操作所需要的时间，也让我们可以将中间结果进行保存。

```c++
bool isMatch(const string s, const string p) {
    vector<vector<bool>> dp(s.length() + 1, vector<bool>(p.length() + 1, false));
    dp[s.length()][p.length()] = true;
    for (int i = s.length(); i >= 0; --i) {
        for (int j = p.length() - 1; j >= 0; --j) {
            bool first_matched = i < s.length() && (s[i] == p[j] || p[j] == '.');
            if (j+1 < p.length() && p[j+1] == '*') {
                dp[i][j] = dp[i][j+2] || (first_matched && dp[i+1][j]);
            } else {
                dp[i][j] = first_matched && dp[i+1][j+1];
            }
        }
    }
    return dp[0][0];
}
```

