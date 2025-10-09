>群里抽一个人，不送东西，纯抽

# 每日两题
---


# 一、基础题
### 题目：[64位整数乘法](https://ac.nowcoder.com/acm/problem/50908)

### 思路：
一道龟速乘模板。不过因为 $1 \leq a,b,p \leq 10^{18}$ ,所以c++直接开 `__int128_t` 计算即可。

可能需要的知识：
- 快速幂&龟速乘&快速乘：学习网站：[(洛谷)快速幂&龟速乘&快速乘](https://www.luogu.com/article/rdvf1eo7)，B站：[快速幂都能做什么？小小的算法也有大大的梦想【五点七边】](https://www.bilibili.com/video/BV16Z4y1M7y1/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O(1)**

```cpp
#include <bits/stdc++.h>
using namespace std;

// 64位整数乘法，防止溢出
void solve() {
    int64_t a, b, p;
    cin >> a >> b >> p;
    // 使用 __int128_t 进行乘法，防止溢出
    __int128_t res = (__int128_t)a * (__int128_t)b % (__int128_t)p;
    cout << (int64_t)res << endl;
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

# 二、提高题
### 题目：[这河里吗？](https://ac.nowcoder.com/acm/problem/220705)

### 思路：
本题是判断一个 9x9 的数独矩阵是否合法。数独的合法条件是每一行、每一列、每个 $3\times3$ 小方格都包含 $1\sim9$ 的所有数字且不重复。由于每行、每列、每个小方格的数字和都应为 $45$（即 $1+2+\cdots+9$），可以通过判断每行、每列、每个小方格的数字和是否为 $45$ 来快速判断合法性。只要有一处不满足条件，就输出 "NO"，否则输出 "YES"。

### 代码(c++)：
时间复杂度 **O(81)**

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> sudoku(10, vector<int>(10, 0));
const int SUM = 45;

// 检查子矩阵内数字和是否为45
bool check(int sx, int sy, int ex, int ey) {
    int tmp = 0;
    for (int i = sx; i <= ex; i++) {
        for (int j = sy; j <= ey; j++) {
            tmp += sudoku[i][j];
        }
    }
    return tmp == SUM;
}

// 检查每一列
bool check_col() {
    for (int i = 1; i <= 9; i++) {
        if (!check(1, i, 9, i)) {
            return false;
        }
    }
    return true;
}

// 检查每一行
bool check_row() {
    for (int i = 1; i <= 9; i++) {
        if (!check(i, 1, i, 9)) {
            return false;
        }
    }
    return true;
}

// 检查每个3x3小方格
bool check_block() {
    for (int i = 1; i <= 7; i += 3) {
        for (int j = 1; j <= 7; j += 3) {
            if (!check(i, j, i + 2, j + 2)) {
                return false;
            }
        }
    }
    return true;
}

void solve() {
    for (int i = 1; i <= 9; i++) {
        for (int j = 1; j <= 9; j++) {
            cin >> sudoku[i][j];
        }
    }
    if (check_row() && check_col() && check_block()) {
        cout << "YES\n";
    } else {
        cout << "NO\n";
    }
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

