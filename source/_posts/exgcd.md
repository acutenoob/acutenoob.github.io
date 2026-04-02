---
title: exgcd
date: 2026-03-31 20:01:18
tags: [模板, 数论]
---

# 实现

```cpp
int exgcd(int a, int b, int &x, int &y) {
    if (b == 0) return x = 1, y = 0, a;
    int r = exgcd(b, a % b, x, y);
    tie(x, y) = make_tuple(y, x - (a / b) * y);
    return r;
}
```

# 原理

要求 $ax + by = (a,b)$ 的一组特解。

先通过辗转相除法得出 $\gcd$，最后 $a = (a,b),\ b = 0$，此时显然可以令 $x = 1,\ y = 0$，然后回溯。

有

$$
\left\{
\begin{aligned}
ax_1 + by_1 &= (a,b) \\
bx_2 + cy_2 &= (b,c)
\end{aligned}
\right.
$$

其中 $c = a - \lfloor \frac a b \rfloor \cdot b$。

依据 $(a,b) = (b, a - k b)$（$k \in \mathbb{Z}$），有 $(a,b) = (b,c)$。

于是

$$
ax_1 + by_1 = bx_2 + (a - \lfloor \frac a b \rfloor \cdot b) y_2
$$

变形得到

$$
ax_1 + by_1 = a y_2 + b \left( x_2 - \lfloor \frac a b \rfloor \cdot y_2 \right)
$$

比较系数得

$$
\left\{
\begin{aligned}
x_1 &= y_2 \\
y_1 &= x_2 - \lfloor \frac a b \rfloor \cdot y_2
\end{aligned}
\right.
$$

即可回溯。

得到的 $x, y$ 是其中一组解。

设 $x_0, y_0$ 是其中一组解，则所有解为

$$
\left( x_0 + k \cdot \frac{b}{(a,b)},\ y_0 - k \cdot \frac{a}{(a,b)} \right), \quad k \in \mathbb{Z}
$$

## 个人原型

```cpp
int exgcd(int a, int b, int &x, int &y) {
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);  // 通过传递引用，此时 x, y 为下层结果
    int temp = x;                   // 上述递推公式
    x = y;
    y = temp - (a / b) * y;
    return r;
}
```

[参考文章](https://www.luogu.com/article/vjrpwen1)
