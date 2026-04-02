---
title: 中国剩余定理
date: 2026-04-01 18:18:15
tags: [模板, 数论]
---

# 实现

```cpp
long long crt(vector<array<long long, 2>> a) {
    long long n = 1, res = 0;
    for (int i = 0; i < a.size(); i++)
        n *= a[i][1];
    for (int i = 0; i < a.size(); i++) {
        long long m = n / a[i][1], b, y;
        exgcd(m, a[i][1], b, y);
        res = (res + m * b * a[i][0] % n) % n;
    }
    return (res % n + n) % n;
}
```

# 原理

变量 $x$ 的一组同余式称为 **同余方程组**：

$$
f_j(x) \equiv 0 \pmod{m_j}, \quad 1 \le j \le k
$$

若整数 $c$ 同时满足：

$$
f_j(c) \equiv 0 \pmod{m_j}, \quad 1 \le j \le k
$$

则称 $c$ 为 **同余方程组的解**。

同余类 $c \mod m$，其中 $m = [m_1, \dots, m_k]$，任意一个整数都是解，写作：

$$
x \equiv c \pmod{m}
$$

设 $m_1, \dots, m_k$ 是两两互素的正整数，则对任意整数 $a_1, \dots, a_k$，同余方程组：

$$
x \equiv a_j \pmod{m_j}, \quad 1 \le j \le k
$$

有解。设：

$$
c = M_1 M_1^{-1} a_1 + \dots + M_k M_k^{-1} a_k
$$

其中：

- $m = m_1 \cdot m_2 \cdots m_k$
- $M_i = \frac{m}{m_i}$
- $M_i^{-1}$ 满足 $M_i M_i^{-1} \equiv 1 \pmod{m_i}$

## 证明

显然 $(M_j, m_j) = 1$，且当 $i \neq j$ 时 $m_i \mid M_j$。

由：

$$
c = M_1 M_1^{-1} a_1 + \dots + M_k M_k^{-1} a_k
$$

可得：

$$
c \equiv M_j M_j^{-1} a_j \pmod{m_j} \equiv a_j \pmod{m_j}
$$

因此，同余方程组的解为：

$$
x \equiv c \pmod{m}
$$

## 个人原型

```cpp
long long crt(vector<array<long long, 2>> a) {
    long long n = 1, res = 0;
    for (int i = 0; i < a.size(); i++)
        n *= a[i][1];
    for (int i = 0; i < a.size(); i++) {
        long long m = n / a[i][1];
        long long b, y;
        exgcd(m, a[i][1], b, y);
        // (m, a[i][1]) = 1 时，有 b * m + y * a[i][1] = 1
        // 推出 b * m % a[i][1] = 1
        res = (res + m * b % n * a[i][0]) % n;
    }
    return (res % n + n) % n; // 注意 b 可能是负数，导致 res 为负
}
```