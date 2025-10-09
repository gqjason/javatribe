>我要悄悄的睡 然后半夜叫醒所有人

# 每日两题
---


# 一、基础题
### 题目：[P1014 [NOIP 1999 普及组] Cantor 表](https://www.luogu.com.cn/problem/P1014#ide)

### 思路：
以对角线枚举为核心：第 k 条对角线上共有 k 个元素，序号累加为 1, 1+2, 1+2+3, ...
依次从 n 中减去 1,2,3,...，直到 n ≤ k，此时 k 为所在对角线编号，n 为该对角线内的位置 r（1-based）。
若 k 为偶数，则对角线从左下到右上：分子 = r，分母 = k - r + 1；
若 k 为奇数，则对角线从右上到左下：分子 = k - r + 1，分母 = r。

### 代码(c++)：
时间复杂度 **O($\sqrt n$)**

```cpp
#include <iostream>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    long long n;
    if (!(cin >> n)) return 0;

    long long k = 1;  // 所在对角线编号
    while (n > k) {
        n -= k;
        ++k;
    }
    // 此时 n 为该对角线内的位置 r（1-based）

    long long num, den;
    if (k % 2 == 0) {
        // 偶数对角线：从左下到右上
        num = n;
        den = k - n + 1;
    } else {
        // 奇数对角线：从右上到左下
        num = k - n + 1;
        den = n;
    }

    cout << num << '/' << den << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[P8597 [蓝桥杯 2013 省 B] 翻硬币](https://www.luogu.com.cn/problem/P8597)

### 思路：
将初始串 a 通过操作转成目标串 b。一次操作会同时翻转相邻的两枚硬币 i 和 i+1。
贪心从左到右扫描：当发现 a[i] != b[i] 时，必须在位置 i 执行一次操作翻转 i 与 i+1，使第 i 位与 b 对齐。因为后续任何选择都无法再只改变第 i 位而不破坏已匹配前缀。
扫描到倒数第二位后，检查是否已与 b 完全一致；若末位仍不相同则无解，输出 -1。
每个位置的操作次数被唯一确定，因而总步数最少。时间复杂度 O(n)。

### 代码(c++)：
时间复杂度 O($n$)

```cpp
#include <iostream>
#include <string>
using namespace std;

static inline char toggle(char c) {
    return c == 'o' ? '*' : 'o';
}

void solve() {
    string a, b;
    cin >> a >> b;

    if (a.size() != b.size()) {
        cout << -1 << '\n';
        return;
    }

    long long ops = 0;
    int n = static_cast<int>(a.size());

    for (int i = 0; i + 1 < n; ++i) {
        if (a[i] != b[i]) {
            a[i] = toggle(a[i]);
            a[i + 1] = toggle(a[i + 1]);
            ++ops;
        }
    }

    if (a == b) cout << ops << '\n';
    else cout << -1 << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    solve();
    return 0;
}
```

