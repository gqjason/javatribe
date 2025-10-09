>吾日三省吾身：吾是不是太客气了 吾是不是给他脸了 吾是不是该动手了

# 每日两题
---


# 一、基础题
### 题目：[小苯的洞数构造](https://ac.nowcoder.com/acm/problem/296614)

### 思路：
- 洞数定义：0/4/6/9 各 1 洞，8 有 2 洞，其他为 0。
- 为使结果最小，先最小化位数：尽量用 8（每位 2 洞）。
- 若 k 为奇数，需要一个 1 洞数字补齐奇偶，放在最高位选最小的 4（0 不能做首位，4 比 6/9 小）；其余用 8 填充。
- 特判：k=0 输出 1（最小正整数且 0 洞），k=1 输出 4。

### 代码(c++)：
时间复杂度 **O(k)**

```cpp
#include <iostream>
using namespace std;

void solve() {
    long long k;
    cin >> k;

    if (k == 0) {
        cout << 1 << '\n';
        return;
    }
    if (k == 1) {
        cout << 4 << '\n';
        return;
    }

    if (k % 2 == 1) cout << 4;
    for (long long i = 0; i < k / 2; ++i) cout << 8;
    cout << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```

# 二、提高题
### 题目：[P2004 领地选择](https://www.luogu.com.cn/problem/P2004)

### 思路：
- 用二维前缀和快速求任意 c×c 子矩形之和。
- 前缀和定义：s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j]，读入时同步构建。
- 枚举所有右下角在 (i,j) 且 i≥c、j≥c 的位置，计算 sum = s[i][j] - s[i-c][j] - s[i][j-c] + s[i-c][j-c]。
- 维护最大值与对应左上角坐标 (i-c+1, j-c+1)，相同则保留先出现的。

### 代码(c++)：
时间复杂度 O($n \times m$)

```cpp
#include <iostream>
#include <vector>
#include <limits>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m, c;
    cin >> n >> m >> c;

    vector<vector<long long>> s(n + 1, vector<long long>(m + 1, 0));

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            long long val;
            cin >> val;
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + val;
        }
    }

    long long best = numeric_limits<long long>::min();
    int best_x = 1, best_y = 1;

    for (int i = c; i <= n; ++i) {
        for (int j = c; j <= m; ++j) {
            long long sum = s[i][j] - s[i - c][j] - s[i][j - c] + s[i - c][j - c];
            if (sum > best) {
                best = sum;
                best_x = i - c + 1;
                best_y = j - c + 1;
            }
        }
    }

    cout << best_x << ' ' << best_y << '\n';
    return 0;
}
```

