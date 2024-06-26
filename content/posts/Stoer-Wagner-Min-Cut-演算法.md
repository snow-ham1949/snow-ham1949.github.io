---
title: Stoer Wagner Min Cut 演算法
date: 2022-10-07 22:24:23
tags: [flow]
---

## 定義

給一個無向連通圖，去掉一個邊集合之後可以使得圖變成兩個連通分量，這個邊集合就叫做割集。

最小割的意思就是找到一個權重和最小的割集。

## Stoer Wagner 演算法

我不會證明，所以直接講求最小割的演算法。（ps. 如果有人會的話可以教我 ><）

```md
全局最小值為 INT_MAX
做 n - 1 次「合併」下面的 3 點直到整個圖變成整個連通
  1. 固定點 s 用 prim 做最大生成樹，並記錄最後擴展的兩個點（也可以想成是最後的點+最後的邊）
  2. 如果跟最後擴展的點連接的所有邊權和 < 全局最小值就更新
  3. 合併最後擴展的那兩個頂點為一個點（我的做法是直接合併邊）
```

## 模板

```cpp
struct SW_Min_Cut {  
    static const int maxn = 500 + 5;
    int edge[maxn][maxn];
    int n;
    int vis[maxn], del[maxn], weight[maxn];

    void Init(int _n)
    {   
        memset(edge, 0, sizeof(edge));
        memset(del, 0, sizeof(del));
        n = _n;
    }
    
    void AddEdge(int u, int v, int w)
    {   
        edge[u][v] += w;
        edge[v][u] += w;
    }

    void Search(int &s, int &t)
    {   
        memset(vis, 0, sizeof(vis));
        memset(weight, 0, sizeof(weight));
        s = t = -1;
        while (true)
        {   
            int mx = -1, cur = 0;
            for (int i = 0; i < n; i++)
            {   
                if (!del[i] && !vis[i] and mx < weight[i])
                {   
                    cur = i, mx = weight[i];
                }
            }
            if (mx == -1) break;
            vis[cur] = 1;
            s = t, t = cur;
            for (int i = 0; i < n; i++)
            {   
                if (!vis[i] && !del[i]) weight[i] += edge[cur][i];
            }
        }
    }

    int Solve()
    {   
        int res = INT_MAX;
        for (int i = 0, x, y; i < n - 1; i++)
        {   
            Search(x, y);
            res = min(res, weight[y]);
            del[y] = 1;
            for (int j = 0; j < n; j++)
            {   
                edge[x][j] = (edge[j][x] += edge[y][j]);
            }
        }
        return res;
    }
} graph;
```
