>真的很累，如果没有文字记载，我还以为金字塔是我今天盖起来的。

# 每日两题
---


# 一、基础题
### 题目：[移植](https://ac.nowcoder.com/acm/problem/296306)

### 思路：
不断循环模拟计算判断即可。

### 代码(c++)：
时间复杂度 **O($\log \log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

// 统计 n 的二进制表示中 1 的个数
int countOnes(int n) {
    int cnt = 0;
    while (n) {
        cnt += (n % 2 == 1);
        n /= 2;
    }
    return cnt;
}

// 统计 n 的二进制表示中 0 的个数（包括前导 0）
int countZeros(int n) {
    int cnt = 0;
    while (n) {
        cnt += (n % 2 == 0);
        n /= 2;
    }
    return cnt + 1;
}

void solve() {
    int n;
    cin >> n;
    while (true) {
        int prev = n;
        n = countZeros(countOnes(n));
        if (prev == n) break;
    }
    cout << n << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

# 二、提高题
### 题目：[B4091 [CSP-X2020 山东] 分糖果](https://www.luogu.com.cn/problem/B4091)

### 思路：

模拟暴力即可(第一眼还以为贪心题🤡)(暴力才是最叼的☝🤓)。进行k次循环，每次循环都对比相邻的`评分`，如果当前孩子的`评分`比相邻的多，则`糖果`要比相邻的多。循环直到所有孩子的`糖果`分配满足条件。因为`评分`最大为100，所最多循环100次，最多为 $100 \times 1e5 = 1e7$，小于时间限制。
### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
using namespace std;

// 定义结构体 Node，表示每个孩子
struct Node {
    int l, r, val, cnt; // l: 左邻居编号, r: 右邻居编号, val: 评分, cnt: 糖果数
};

void solve() {
    int n;
    cin >> n;
    vector<Node> a(n + 2); // 存储所有孩子的信息，1~n编号

    // 读入评分，并初始化左右邻居和糖果数
    for (int i = 1; i <= n; i++) {
        cin >> a[i].val;
        a[i].l = (i == 1 ? n : i - 1); // 环形左邻居
        a[i].r = (i == n ? 1 : i + 1); // 环形右邻居
        a[i].cnt = 1; // 初始每人1颗糖
    }

    bool changed = true;
    // 不断循环，直到所有孩子的糖果分配满足条件
    while (changed) {
        changed = false;
        for (int i = 1; i <= n; i++) {
            int l = a[i].l, r = a[i].r;
            // 如果当前孩子评分比左邻居高且糖果数不多于左邻居，则糖果数加1
            if (a[i].val > a[l].val && a[i].cnt <= a[l].cnt) {
                a[i].cnt = max(a[i].cnt, a[l].cnt + 1);
                changed = true;
            }
            // 如果当前孩子评分比右邻居高且糖果数不多于右邻居，则糖果数加1
            if (a[i].val > a[r].val && a[i].cnt <= a[r].cnt) {
                a[i].cnt = max(a[i].cnt, a[r].cnt + 1);
                changed = true;
            }
        }
    }

    int ans = 0;
    // 统计所有孩子的糖果总数
    for (int i = 1; i <= n; i++) {
        ans += a[i].cnt;
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

