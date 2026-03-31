---
title: exgcd
date: 2026-03-31 20:01:18
tags: 模板
---

# 实现 
```cpp
int exgcd(int a,int b,int &x,int &y)
{
    if(b == 0) return x = 1,y = 0,a;
    int r = exgcd(b,a % b,x,y);
    tie(x,y) = make_tuple(y,x - (a/b) * y);
    return r;
}
```
# 原理

要求 $ax + by = (a,b)$ 的一组特解

先通过辗转相处法得出 gcd 最后 $a = (a,b),b = 0$ 显然此时可以令 $x = 1,y = 0$ 然后回溯
有
$$
\left\{\begin{array}{l}
 ax_1 + bx_1 = (a,b)
\\ bx_2 + cy_2 = (b,c)
\end{array}\right.
$$
其中 $c = (a,b) = a - a / b * b$

依据  $(a,b) = (b,a - kb)\ 其中\ k \in \mathbb{Z}$

$(a,b) = (b,c)$

有 $ ax_1 + by_1 = bx_2 + (a - a / b * b)y_2 $

变形得到 $ ax_1 + by_1 = ay_2 + b(x_2 - (a / b) \times y_2)$

$$
\left\{\begin{array}{l}
x_1 = y_2
\\ y_1 = x_2 - (a / b)y_2
\end{array}\right.
$$
即可回溯

得到的x , y是其中一组解

设 $x_0,y_0$是其中一组解 则所有解为 $(x_0 + k\frac{b}{(a,b)},y_0 - k\frac{a}{(a,b)})\ k\in \mathbb{Z}$
## 个人原型

```cpp
int exgcd(int a,int b,int &x,int &y)
{
    if(b == 0)
    {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b,a % b,x,y)  // 通过传递指针 此时x,y为下层结果
    int temp = x; //上述递推公式
    x = y; 
    y = temp - (a / b) * y;
    return r;
} 
```
[参考文章](https://www.luogu.com/article/vjrpwen1)

