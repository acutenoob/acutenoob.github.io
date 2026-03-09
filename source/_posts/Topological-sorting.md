---
title: 拓扑排序
date: 2026-03-09 22:36:58
tags: 模板
---


> 拓扑排序的目标是将所有节点排序，使得排在前面的节点不能依赖于排在后面的节点．

取出一个入度为 0 的点

删除这个点的所有边 更新 这些边所连点的入度

[codeforces](https://codeforces.com/contest/2143/problem/C)

```cpp
#include <array>
#include <iostream>
#include <stack>
#include <utility>
#include <vector>
using namespace std;


void solve()
{
    int n;
    cin >> n;
    vector<vector<int> > t(n + 1);
    vector<int> in(n + 1);
    vector<int> res(n + 1);
    for(int i = 1;i < n;i++)
    {
        int u,v,x,y;
        cin >> u >> v >> x >> y;
        if(u > v)
            swap(u,v);
        if(x < y)
        {
            t[u].push_back(v);
            in[v]++;
        }
        else
        {
            t[v].push_back(u);
            in[u]++;
        }
    }
    stack<int> ni;
    for(int i = 1;i <= n;i++)
        if(in[i] == 0)
            ni.push(i);
    int c = 1;
    while(ni.size() != 0)
    {
        int i = ni.top();
        ni.pop();
        res[i] = c;
        for(int j = 0;j < t[i].size();j++)
        {
            in[t[i][j]]--;
            if(in[t[i][j]] == 0)
                ni.push(t[i][j]);
        }
        c++;
    }
    for(int i = 1;i <= n;i++)
        cout << res[i] << ' ';
    cout << '\n';
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    cout.unsetf(ios::fixed);
    cout.precision(20);
    int t;
    cin >> t;
    while(t--)
        solve();
}
```
