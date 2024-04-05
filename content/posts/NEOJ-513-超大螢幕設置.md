---
title: NEOJ 513 超大螢幕設置
date: 2022-10-04 15:20:17
tags: [NEOJ, monotonic stack]
---

## 題目

[link](https://neoj.sprout.tw/problem/513/)

## 作法

是單調隊列的經典題目，只是這題變成要往左往右看，看到第一個比它小的就停止，這樣就會知道以它的高度設置螢幕的話最寬能夠到多少。這邊用 vector 實作 stack 比較好寫（我看了別人的 code 覺得好棒），只是我不太習慣它 stack 的寫法，所以把 stack 存的值改成 {value, id} 這樣。

然後記得要開 long long ，我 WA 了好多次 QQ

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
#define int ll
#define SZ(a) (int)(a).size()
#define endl '\n'
#define X first
#define Y second

const int mxn = 2e5 + 5;
int a[mxn], lhs[mxn], rhs[mxn], ans[mxn];

void solve()
{
  int n; cin >> n;
  for (int i = 1; i <= n; i++) cin >> a[i];
  
  vector<pll> stk(1, {0ll, 0ll}); // store {value, id}
  for (int i = 1; i <= n; i++) {
    while (a[i] <= stk.back().X) stk.pop_back();
    lhs[i] = i - stk.back().Y;
    stk.push_back({a[i], i});
  }
  while (!stk.empty()) stk.pop_back();

  stk.push_back({0, n + 1});
  for (int i = n; i >= 1; i--) {
    while (a[i] <= stk.back().X) stk.pop_back();
    rhs[i] = stk.back().Y - i - 1;
    stk.push_back({a[i], i});
  }

  for (int i = 1; i <= n; i++) {
    ans[i] = a[i] * (lhs[i] + rhs[i]);
  }
  cout << *max_element(ans + 1, ans + n + 1) << endl;
}

signed main() {
  ios::sync_with_stdio(0);cin.tie(0);
  int t = 1;
  // cin >> t;
  while (t--)
  {
    solve(); 
  }
  return 0;
}
```
