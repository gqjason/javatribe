>你们只看到我每天搞抽象，精神诡异，发无营养微博，吃垃圾食品晚睡晚起的样子，却从没看到我努力生活，熬夜加班，认真与人交流，坚持学习的样子。你们看不到，我也没干过。

# 每日两题
---


# 一、基础题
### 题目：[凌波微步](https://ac.nowcoder.com/acm/problem/14346)

### 思路：

看似求最大上升子序列，实际是求数组去重后的长度🤣 (导致我还WA了一发🤬)。
没什么好说的，直接用`set`模拟即可。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T; 
    cin >> T;
    while (T--) {
        int n;
        cin >> n;
        set<int> distinct;
        for (int i = 0; i < n; i++) {
            int num;
            cin >> num;
            distinct.insert(num);
        }
        cout << distinct.size() << '\n'; 
    }
    return 0;
}

```

# 二、提高题
### 题目：[P12836 [蓝桥杯 2025 国 B] 翻倍](https://www.luogu.com.cn/problem/P12836)

### 思路：
因为 $A_i$ 最大为 $2^{32}$ 且 $1 \leq n \leq 2e5$，因此直接模拟会超出`int`范围甚至`long long`，所以，考虑贪心。
遍历数组，对于每个位置，如果当前数比前一个数大，则不断将前一个数翻倍直到不小于当前数，记录所需的翻倍次数，并累加到答案中。如果当前数不大于前一个数，则翻倍次数清零。最终输出所有位置的翻倍次数之和。

### 代码(c++)：
时间复杂度 **O($n \log A$)**，其中 $n$ 为数组长度，$A$ 为数组中的最大值。

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }

    int ans = 0, d = 0;
    for (int i = 2; i <= n; i++) {
        if (a[i] > a[i - 1]) {
            int diff = 0;
            int cur = a[i - 1];
            while (cur < a[i]) {
                cur *= 2;
                diff++;
            }
            d += diff;
        } else {
            d = 0;
        }
        ans += d;
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

