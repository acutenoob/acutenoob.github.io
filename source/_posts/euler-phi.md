---
title: 欧拉函数
date: 2026-03-12 10:47:30
tags: 模板 unsolve
---

欧拉函数（Euler's totient function），即 $\varphi(n)$，表示的是小于等于 n 和 n 互质的数的个数．

# 实现

```cpp
int euler(int n)
{
    int res = n;
    for(int i = 2;i * i <= n;i++)
    {
        if(n % i == 0)
        {
            res = res / i * (i - 1);
            while(n % i == 0)
                n /= i;
        }
    }
    if(n > 1)
        res = res / n * (n - 1);
    return res;
}
```
# 原理

> 前方的道路,以后再来探索吧 未来的我会告诉我的

26/3/12 刚刚买了 初等数论 书的 acutenoob
