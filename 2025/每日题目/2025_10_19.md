>我知道如何预测地震了，八个方向各摆一台手机，哪个跳转csdn了，就是哪块震了

# 每日两题
---


# 一、基础题
### 题目：[P1003 [NOIP 2011 提高组] 铺地毯](https://www.luogu.com.cn/problem/P1003)

### 思路：
输入`(x,y)`后，直接遍历枚举是否在每块的地毯内即可

### 代码(c++)：
时间复杂度 **O($n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<tuple<int, int, int, int>> carpets(n);
    for (int i = 0; i < n; ++i) {
        int x, y, w, h;
        cin >> x >> y >> w >> h;
        carpets[i] = {x, y, x + w, y + h};
    }
    int fx, fy;
    cin >> fx >> fy;
    int res = -1;
    for (int i = 0; i < n; ++i) {
        auto [x1, y1, x2, y2] = carpets[i];
        if (fx >= x1 && fx <= x2 && fy >= y1 && fy <= y2) {
            res = i + 1;
        }
    }
    cout << res << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

# 二、提高题
### 题目：[P10387 [蓝桥杯 2024 省 A] 训练士兵](https://www.luogu.com.cn/problem/P10387)

### 思路：
一道贪心题。
先把士兵按训练的次数排序，让士兵单独训练一次，如果成本高于报团训练的成本`S`，我们就让剩下的士兵报团训练，否则就让士兵单独训练。

**⚠⚠⚠:**
- 数据范围有 $10^{10}$，记得开`long long`。

### 代码(c++)：
时间复杂度 **O($n \log n$)**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Node {
    long long p, c;
};

void solve() {
    int n;
    long long s;
    cin >> n >> s;
    vector<Node> a(n);
    long long total = 0;
    for (int i = 0; i < n; ++i) {
        cin >> a[i].p >> a[i].c;
        total += a[i].p;
    }

    sort(a.begin(), a.end(), [](const Node &a1, const Node &a2) {
        return a1.c < a2.c;
    });

    long long ans = 0, used = 0;
    for (int i = 0; i < n; ++i) {
        if (total >= s) {
            ans += (a[i].c - used) * s;
            total -= a[i].p;
            used += a[i].c - used;
        } else {
            ans += (a[i].c - used) * a[i].p;
            total -= a[i].p;
        }
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

