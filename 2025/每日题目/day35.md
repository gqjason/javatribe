>但凡用钱能解决的事情，我都解决不了。

# 每日两题
---


# 一、基础题
### 题目：[P1031 [NOIP 2002 提高组] 均分纸牌](https://www.luogu.com.cn/problem/P1031)

### 思路：

本题要求将若干堆纸牌通过相邻两堆之间的移动，使每堆纸牌数量相等。每次操作可以将一堆纸牌的一部分移动到相邻的下一堆。我们可以先计算出每堆应有的平均纸牌数，然后从左到右遍历每一堆，如果当前堆纸牌数不等于平均数，则将多余或不足的纸牌移动到下一堆，并计数操作次数。最终操作次数即为答案。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

void go() {
    int n;
    cin >> n;
    vector<int> a(n + 1);
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        sum += a[i];
    }
    int ave = sum / n;
    int ans = 0;
    for (int i = 1; i < n; ++i) {
        if (a[i] != ave) {
            int diff = a[i] - ave;
            a[i + 1] += diff;
            ans++;
        }
    }
    cout << ans << endl;
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    while (t--) {
        go();
    }
    return 0;
}
```

### 题目：[好数组](https://ac.nowcoder.com/acm/problem/298866)

### 思路：

从任意位置切割一个含有01的环形数组并将数组中连续相同的元素合并后，可以得出 `0,1,0`, `1,0,1`, `0,1`, `1,0` 4种结果。 
因此，我们可以将数组`a`中连续相同的元素合并后，如果数组长度不超过3，则为好数组。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> arr;
    for (int i = 0; i < n; ++i) {
        int x;
        cin >> x;
        if (arr.empty() || arr.back() != x) {
            arr.push_back(x);
        }
    }
    cout << (arr.size() <= 3 ? "YES" : "NO") << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

