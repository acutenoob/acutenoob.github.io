---
title: tarjan
date: 2026-03-09 22:08:20
tags: 模板
---

[洛谷P3388](https://www.luogu.com.cn/problem/P3388)

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;
vector<int> dfn; // 遍历到的次序
vector<int> low; // 不经过他的 能达到的最浅位置
vector<bool> f; 
vector<bool> f2;
vector<int> pts;
vector<vector<int>> t;
int res = 0;
int idx;
void tj(int n,int p)
{
    f[n] = 1;
    low[n] = dfn[n] = ++idx;
    int c = 0;
    for(int i = 0;i < t[n].size();i++)
    {
        if(t[n][i] == p)
            continue;
        if(!f[t[n][i]])
        {
            c++;
            tj(t[n][i],n);
            low[n] = min(low[n],low[t[n][i]]); 
            if(n != p && low[t[n][i]] >= dfn[n] && !f2[n]) // 如果 low[t[n][i]] >= dfn[n] 说明 他回不去 所以他爹是割点
            {
                f2[n] = 1;
                res++;
                pts.push_back(n); 
            }
        }
        else 
            low[n] = min(low[n],dfn[t[n][i]]); // 防止通过邻居间接通过他爹 所以 这里是 dfn
    }
    if(p == n && c >= 2 && !f2[n]) // 特判初始点 向下能搜索两次 说明 这搜搜的两条是断开的
    {
        f2[n] = 1;
        res++;
        pts.push_back(n); 
    }
}
void solve()
{
    int n,m;
    cin >> n >> m;
    t.assign(n + 1,{});
    low.assign(n + 1,0);
    dfn.assign(n + 1,0);
    f.assign(n + 1,0);
    f2.assign(n + 1,0);
    for(int i = 0;i < m;i++)
    {
        int u,v;
        cin >> u >> v;
        t[u].push_back(v);
        t[v].push_back(u);
    }
    for(int i = 1;i <= n;i++)
    {
        if(f[i])
            continue;
        idx = 0;
        tj(i,i);
    }
    cout << res << '\n';
    sort(pts.begin(),pts.end());
    for(int i = 0;i < res;i++)
        cout << pts[i] << ' ';
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    cout.unsetf(ios::fixed);
    cout.precision(20);
    int t = 1;
    //cin >> t;
    while(t--)
        solve();
}
```