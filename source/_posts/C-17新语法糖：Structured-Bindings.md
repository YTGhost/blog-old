---
title: C++17新语法糖：Structured Bindings
date: 2021-01-06 21:08:49
tags:
- C++
categories:
- 学习笔记
---
今天写题的时候发现C++17有一种新的语法糖：Structured Bindings，能让我们更方便地一次定义多个对象，一起来看看吧。
<!--more-->
## 简要介绍
假如有这么一个函数：
```c++
std::tuple<char, int, bool> mytuple()
{
    char a = 'a';
    int i = 123;
    bool b = true;
    return std::make_tuple(a, i, b);
}
```
我们可以看到，这个函数返回了三个不同类型的变量，在C++17以前，我们可以这样来获得这三个变量。
```c++
char a;
int i;
bool b;

std::tie(a, i, b) = mytuple()
```
这样用的话我们得事先定义好对应的变量以及它们的类型，但如果使用新的语法糖的话:
```c++
auto [a, i, b] = mytuple();
```
那么这个语法糖都有哪些使用场景呢？来看看吧
## 直接赋值
在之前，我们如果要赋予三个变量不同类型的值，我们可能会这样
```c++
auto a = 'a';
auto i = 123;
auto b = true;
```
但有了这个语法糖后，我们可以这样
```c++
auto [a, i, b] = tuple('a', 123, true);
```
## 遍历一个复合结构
在之前我们去遍历个map可能会这样写
```c++
for (const auto& entry : mymap) {
    auto& key = entry.first;
    auto& value = entry.second;
    // 做一些处理的逻辑...
}
```
但现在我们可以这样了
```c++
for (const auto& [key, value] : mymap) {
    // 可以直接拿到key和value去做处理...
}
```