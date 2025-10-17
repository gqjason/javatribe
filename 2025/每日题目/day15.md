>大家好，耽误大家半分钟，我小时候从没好好的过一次生日，今天是10月17日，不是我的生日，我只是想耽误大家的时间。

# 每日两题
---

# 一、基础题
### 题目：[P1538 迎春舞会之数字舞蹈](https://www.luogu.com.cn/problem/P1538)

### 思路：
小模拟。简单题简单做，不要把代码搞复杂。注意，数字之间要有空格。
以数字8为例：
" - " .....1
"| |" .....2
" - " .....3
"| |" .....4
" - " .....5
对于所有行，我需要将中间的字符打印n次，两边不动；当遇到偶数行时，我们需要将偶数行打印n行。

### 代码(c++)：
时间复杂度 **O($k \cdot len(s)$)**, 其中`k`为要摆出数字的大小，`s`为输入的字符串。

```cpp
#include <iostream>
#include <string>
#include <array>

using namespace std;

static const array<array<string, 5>, 10> DIGITS = {{
    { 
        " - ",
        "| |", 
        "   ", 
        "| |", 
        " - " 
    }, // 0
    { 
        "   ",
        "  |",
        "   ",
        "  |",
        "   " 
    }, // 1
    { 
        " - ",
        "  |",
        " - ",
        "|  ", 
        " - " 
    }, // 2
    { 
        " - ",
        "  |",
        " - ",
        "  |", 
        " - " 
    }, // 3
    { 
        "   ",
        "| |",
        " - ", 
        "  |", 
        "   " 
    }, // 4
    { 
        " - ",
        "|  ", 
        " - ", 
        "  |", 
        " - "
    }, // 5
    { 
        " - ", 
        "|  ", 
        " - ", 
        "| |", 
        " - " 
    }, // 6
    { 
        " - ", 
        "  |", 
        "   ", 
        "  |", 
        "   " 
    }, // 7
    { 
        " - ", 
        "| |", 
        " - ", 
        "| |", 
        " - " 
    }, // 8
    { 
        " - ",
        "| |", 
        " - ", 
        "  |", 
        " - " 
    }  // 9
}};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    string s;
    if (!(cin >> n >> s)) return 0;

    auto print_row = [&](int row) {
        for (char ch : s) {
            int d = ch - '0';
            const string& cell = DIGITS[d][row];
            cout << cell[0] << string(n, cell[1]) << cell[2] << ' ';
        }
        cout << '\n';
    };

    for (int row = 0; row < 5; ++row) {
        int repeat = (row == 1 || row == 3) ? n : 1;
        for (int r = 0; r < repeat; ++r) {
            print_row(row);
        }
    }

    return 0;
}

```

# 二、提高题
### 题目：[P10902 [蓝桥杯 2024 省 C] 回文数组](https://www.luogu.com.cn/problem/P10902)

### 思路：

我们可以选择$a_{i}$和$a_{i+1}$一起$\pm 1$ 或 只使$a_{i}$加$\pm 1$ 两操作，要使操作数最小化，应尽可能进行前者的操作，所以，考虑贪心。
我们可以初始化一个数组`b`，使 $b_i=a_{n-i+1}-a_{i},(1 \leq i \leq n/2)$。
对于$b_i \times b_{i+1} > 0$ 时，即相邻两项差值为同号，我可以进行 $\min( \lvert b_i \rvert, \lvert b_{i+1} \rvert)$ 次前者操作。如果 $\lvert b_i \rvert \geq \lvert b_{i+1} \rvert$，我们需要进行 $\lvert b_i \rvert - \lvert b_{i+1} \rvert$ 次后者操作；反之，$b_{i+1}$ 操作完后剩下的部分留到 ${b_{i+1},b_{i+2}}$ 再处理即可。
对于$b_i \times b_{i+1} \leq 0$ 时，即相邻两项差值为异号或其中一项为0，我们只能对 $b_i$ 进行 $\lvert b_i \rvert$ 次后者操作。

### 代码(c++)：
时间复杂度 **O($n$)**
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

void solve() {
    int n;
    cin >> n;
    int half = n / 2;
    vector<long long> a(n + 1, 0);
    vector<long long> b(half + 2, 0); // b[1..half], b[half+1] as sentinel 0

    for (int i = 1; i <= n; ++i) cin >> a[i];

    for (int i = 1; i <= half; ++i) {
        b[i] = a[n - i + 1] - a[i];
    }

    long long ans = 0;
    for (int i = 1; i <= half; ++i) {
        if (b[i] * b[i + 1] > 0) {
            if (b[i] > 0) {
                long long d = min(b[i], b[i + 1]);
                ans += llabs(b[i]);
                b[i + 1] -= d;
            } else {
                long long d = max(b[i], b[i + 1]);
                ans += llabs(b[i]);
                b[i + 1] -= d;
            }
        } else {
            ans += llabs(b[i]);
            b[i] = 0;
        }
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

