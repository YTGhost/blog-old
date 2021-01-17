---
title: C++三种比较排序语法
date: 2021-01-17 23:35:48
tags:
- C++
categories:
- 学习笔记
---
分别是重载小于号、自定义比较函数和lambda表达式~
<!--more-->
多关键字排序模板题：[AcWing 429. 奖学金](https://www.acwing.com/problem/content/431/)

## 重载小于号
```c++
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 310;

struct Person
{
    int id, sum, a, b, c;
    bool operator< (const Person& t) const
    {
        if(sum != t.sum) return sum > t.sum;
        if(a != t.a) return a > t.a;
        return id < t.id;
    }
}q[N];

int main()
{
    int n;
    cin >> n;

    for(int i = 1; i <= n; i++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        q[i] = {i, a+b+c, a, b, c};
    }

    sort(q + 1, q + n + 1);

    for(int i = 1; i <= 5; i++)
        cout << q[i].id << " " << q[i].sum << endl;
    return 0;
}
```

## 自定义比较函数
```c++
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 310;

struct Person
{
    int id, sum, a, b, c;
}q[N];

bool cmp(Person &a, Person &b)
{
    if(a.sum != b.sum) return a.sum > b.sum;
    if(a.a != b.a) return a.a > b.a;
    return a.id < b.id;
}

int main()
{
    int n;
    cin >> n;

    for(int i = 1; i <= n; i++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        q[i] = {i, a+b+c, a, b, c};
    }

    sort(q + 1, q + n + 1, cmp);

    for(int i = 1; i <= 5; i++)
        cout << q[i].id << " " << q[i].sum << endl;
    return 0;
}
```

## lambda表达式
```c++
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 310;

struct Person
{
    int id, sum, a, b, c;
}q[N];

int main()
{
    int n;
    cin >> n;

    for(int i = 1; i <= n; i++)
    {
        int a, b, c;
        cin >> a >> b >> c;
        q[i] = {i, a+b+c, a, b, c};
    }

    sort(q + 1, q + n + 1, [](Person& a, Person& b) {
        if(a.sum != b.sum) return a.sum > b.sum;
        if(a.a != b.a) return a.a > b.a;
        return a.id < b.id;
    });

    for(int i = 1; i <= 5; i++)
        cout << q[i].id << " " << q[i].sum << endl;
    return 0;
}
```