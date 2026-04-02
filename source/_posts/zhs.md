---
title: 组合数
date: 2026-03-04 17:43:43
tags: [模板,组合数学]
---

# 实现

```cpp
int ksm(int a,int b)
{
    int res = 1;
    for(;b;a = 1ll * a * a % p,b /= 2)
        if(b & 1)
            res = 1ll * res * a % p;
    return res;
}
void init()
{
    fac[0] = 1;
    for(int i = 1;i < mxsize;i++)
        fac[i] = 1ll * fac[i - 1] * i % p;
    inv[mxsize - 1] = ksm(fac[mxsize - 1],p - 2);  // 求 fac[5000] 的 逆元
    for(int i = mxsize - 1;i >= 1;i--)
        inv[i - 1] = 1ll * inv[i] * i % p; // 1 / n! * n = 1 / (n - 1)!
}
int c(int a,int b)
{
    int res = 1ll * fac[b] * inv[a] % p;
    res = 1ll * res * inv[b - a] % p;
    return res;
}
```

$C^a_b$ = `fac[b] * inv[a] * inv[b - a]`

# 原理

根据

$$a ^ { \varphi (n)} \equiv 1~(mod~n)$$

其中 $\varphi (n)$ 为欧拉函数 当 n 为质数时候 存在 $a ^ {n - 1} \equiv 1~(mod~n)$
