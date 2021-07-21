---
layout: post
read_time: true
show_date: true
title: "CodeForces Round #1550 (A~C)"
date: 2021-07-20
img: posts/cover/warma.jpg
tags: [ACM, CodeForces]
category: theory
author: rivego
description: "me nub."
---

[题目链接](https://codeforces.com/contest/1550)

[TOC]

# A - Find The Array

题意：如果一个数组中任一a~i~满足以下条件之一则称为好数组
1.a<sub>i</sub>>=1
2.a<sub>i</sub>-1或a<sub>i</sub>-2至少一个存在于数组中
给出数组的和$s$，问最小长度的好数组长度为多少
思路：观察后,由等差数列求和公式发现一个长度为$n$的好数组元素之和的范围是[$\frac{n(1+n)}{2}$ ,$n^{2}$]（公差分别为1和2）.所以只需要找到大于等于$s$的第一个$n$。
Code：

```cpp
//不够优雅.jpg
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define gcd __gcd
const int mod = 1e7 + 9;
const int inf = 0x3f3f3f3f;
int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int s;
        cin >> s;
        int ans;
        for (int i = 1; i <= 100; i++)
        {
            if (i * i >= s)
            {
                ans = i;
                break;
            }
        }
        cout << ans << endl;
    }

    return 0;
}
```
# B - Maximum Cost Deletion
题意:给出一个长度为$n$的01字符串$s$与常量$a$,$b$。你需要删除若干次连续子字符串使字符串为空，每删除一次长度为$l$的字符串你将获得$a*l+b$个点数，求能获得的最大点数
思路：因为最终要删除的长度必然是n，所以最终点数与a没有关系，只需要判断b的正负来进行操作。
如果b为正数，那么我们要最大化操作次数使点数最大，即每次删除一个字符
如果b为负数，那么我们要最小化操作次数使点数最大，可以先选择连续次数小的删除，之后剩下的就只有0或1，只需要进行一次删除操作。
Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define gcd __gcd
const int mod = 1e7 + 9;
const int inf = 0x3f3f3f3f;
int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int l, a, b;
        string s;
        cin >> l >> a >> b;
        cin >> s;
        int ans = 0;
        ans += a * l;
        if (b < 0)
        {
            int c1 = 0, c2 = 0;
            for (int i = 0; i < l; i++)
            {
                if (s[i] == '1')
                {
                    int p = i;
                    while (s[p] == '1')
                    {
                        p++;
                    }
                    i = p - 1;
                    c1++;
                }
            }
            for (int i = 0; i < l; i++)
            {
                if (s[i] == '0')
                {
                    int p = i;
                    while (s[p] == '0')
                    {
                        p++;
                    }
                    i = p - 1;
                    c2++;
                }
            }
            // cout << "c1:" << c1 << endl;
            // cout << "c2:" << c2 << endl;
            ans += min(c1, c2) * b + b;
        }
        else
        {
            ans += b * l;
        }
        cout << ans << endl;
    }

    return 0;
}
```
# 	C - Manhattan Subarrays
题意：假设你有两个点 $p=(xp,yp)$ 和 $q=(xq,yq)$. 两点的曼哈顿距离为$d(p,q)=|xp−xq|+|yp−yq|$.如果存在三个点使$d(p,r)=d(p,q)+d(q,r)$，
这个三元组是坏三元组。现在有一个数组 $b_{1},b_{2},…,b_{m}$,如果任选三个元素组成一个三元组$(b_{i},i),(b_{j},j),(b_{k},k)$都不是坏三元组，那么这是一个好数组。求$b$数组的连续子数组为好数组的个数。
思路：不妨设i<j<k，那么由题意可推出这个式子

$$\left | a_{i}-a_{k} \right | \ne \left |a_{i}-a_{j}\right |+\left |a_{j}-a_{k}\right |$$

我们再次假设所选的三个数为递增或递减，发现公式不成立，所以我们选择的连续子数组任选三个数都不能是递增或递减，因此，所选连续子数组最大长度不超过4.
Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define gcd __gcd
const int mod = 1e7 + 9;
const int inf = 0x3f3f3f3f;
int a[200005];
long long d(int x1, int y1, int x2, int y2)
{
    return abs(x1 - x2) + abs(y1 - y2);
}
int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++)
        {
            cin >> a[i];
        }
        if (n == 2)
        {
            cout << 3 << endl;
            continue;
        }
        else if (n == 1)
        {
            cout << 1 << endl;
            continue;
        }
        ll ans = 0;
        ans += 2 * n - 1;
        for (int i = n; i >= 3; i--)
        {
            if (abs(a[i - 2] - a[i]) != abs(a[i - 2] - a[i - 1]) + abs(a[i] - a[i - 1]))
            {
                ans++;
            }
        }
        if (n > 3)
        {
            for (int i = n; i >= 4; i--)
            {
                if (abs(a[i - 2] - a[i]) != abs(a[i - 2] - a[i - 1]) + abs(a[i] - a[i - 1]))
                    if (abs(a[i - 3] - a[i - 1]) != abs(a[i - 3] - a[i - 2]) + abs(a[i - 2] - a[i - 1]))
                    {
                        if (abs(a[i - 3] - a[i]) != abs(a[i - 3] - a[i - 2]) + abs(a[i - 2] - a[i]))
                        {
                            if (abs(a[i - 3] - a[i]) != abs(a[i - 3] - a[i - 1]) + abs(a[i] - a[i - 1]))
                            {
                                ans++;
                            }
                        }
                    }
            }
        }
        cout << ans << endl;
    }
    return 0;
}
```
为什么没有D、E、F？~~（因为不会 理直气壮.jpg）doge~~ 

$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$

$S_{n}=\frac{n \left( a_{1}+a_{n}\right)}{2} $

$S_{n}=\\frac{n \\left( a_{1}+a_{n}\\right)}{2}$

