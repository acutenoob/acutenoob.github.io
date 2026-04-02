---
title: 欧拉函数
date: 2026-03-12 10:47:30
tags: [模板, unsolve, 数论]
---

欧拉函数（Euler's totient function），即 $\varphi(n)$，表示的是小于等于 $n$ 且与 $n$ 互质的数的个数。

# 实现

## 在线

```cpp
int euler(int n)
{
    int res = n;
    for(int i = 2; i * i <= n; i++)
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

## 离线

```cpp
const int mxi = 50000;
int phi[mxi + 1];
bool np[mxi + 1];
vector<int> pm;
void init()
{
    for(int i = 2;i <= mxi;i++)
    {
        if(!np)
            pm.push(i),phi[i] = i - 1;
        for(int j = 0;j < np.size() && i * np[j] <= mxi;j++)
        {
            if(i % pm[j] == 0)
            {
                phi[i * pm[j]] = phi[i] * pm[j];
                break;
            }
            phi[i * pm[j]] = phi[i] * (pm[j] - 1);
        }
    }
}
```

# 原理

> 前方的道路，以后再来探索吧 未来的我会告诉我的

26/3/12 刚刚买了《初等数论》书的 acutenoob

26/3/31  
设 $n = p_1^{\alpha_1} \dots p_m^{\alpha_m}$，则  
$$
\varphi(n) = (p_1 - 1)p_1^{\alpha_1 - 1} \dots (p_m - 1)p_m^{\alpha_m - 1}
 $$

## 个人原型

### 在线
```cpp
int euler(int n)
{
    int res = n;
    for(int i = 2;i * i <= n;i++)
    {
        if(n % i != 0)
            continue;
        res = res / i * (i - 1); 
        while(n % i == 0)
            n /= i;
    }
    if(n > 1)
        res = res / n * (n - 1);
    return res;
}
```

### 离线

```cpp
const int mxn = 100000;
vector<bool> np(mxn + 1);
vector<int> pm;
vector<int> phi(mxn + 1);

void init()
{
    for(int i = 2; i <= mxn; i++)
    {
        if(!np[i])
            pm.push_back(i), phi[i] = i - 1;
        for(int j = 0; j < pm.size() && 1ll * i * pm[j] <= mxn; j++)
        {
            np[i * pm[j]] = 1;
            if(i % pm[j] == 0)
            {
                phi[i * pm[j]] = phi[i] * pm[j]; // 此时能被整除，乘 pm[j]
                break;
            }
            phi[i * pm[j]] = phi[i] * (pm[j] - 1); // 此时 i 不能被 pm[j] 整除，乘 pm[j] - 1
        }
    }
}
```