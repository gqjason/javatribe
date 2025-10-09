>你不妨试着爱上一个复制党，你说什么他就说什么，他从来不会和你唱反调，只要你敢说，他就敢复制。

# 每日两题
---


# 一、基础题
### 题目：[P1015 [NOIP 1999 普及组] 回文数](https://www.luogu.com.cn/problem/P1015)

### 思路：
直接循环模拟相加即可，`M`要用字符串输入。
### 代码(c++)：
时间复杂度 **O($30 \times L$)**，其中 $L$ 为数字的长度。每次最多进行 30 次回文判断和加法操作。
```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<int> arr;
map<char, int> mp = {
    {'0', 0}, {'1', 1}, {'2', 2}, {'3', 3}, {'4', 4}, {'5', 5}, 
    {'6', 6}, {'7', 7}, {'8', 8}, {'9', 9},{'A', 10}, 
    {'B', 11}, {'C', 12}, {'D', 13}, {'E', 14}, {'F', 15}
};//键值对

//反转相加
void bigint_add(vector<int>& num) {
    vector<int> rev_num = num;
    reverse(rev_num.begin(), rev_num.end());
    int carry = 0, len = num.size();
    for (int i = 0; i < len; ++i) {
        int sum = num[i] + rev_num[i] + carry;
        num[i] = sum % n;
        carry = sum / n;
    }
    if (carry > 0) num.push_back(carry);
}

//判断是否回文
bool is_pali(const vector<int>& num) {
    int l = 0, r = num.size() - 1;
    while (l < r) {
        if (num[l] != num[r]) return false;
        ++l; --r;
    }
    return true;
}

void solve() {
    cin >> n;
    string s;
    cin >> s;
    arr.clear();

    //转成直接对应的数值容易计算
    for (char& c : s) {
        if (isalpha(c)) c = toupper(c);
        arr.push_back(mp[c]);
    }

    //不断循环处理判断
    for (int step = 0; step <= 30; ++step) {
        if (is_pali(arr)) {
            cout << "STEP=" << step << endl;
            return;
        }
        bigint_add(arr);
    }
    cout << "Impossible!" << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```


# 二、提高题
### 题目：[小红的不动点权值](https://ac.nowcoder.com/acm/problem/299558)
### 思路：
题目中，不动点的定义为将数组`a`排序后 $a_i=i\ (1 \leq i \leq \text{len}(a))$ ，我们可以发现，要使 $a_i=i$ 成立，前提必须是 $a_k=k\ (1 \leq k \leq i-1)$ 。

- 比如，对于样例 `1 4 2 3` 来说，
包含 `1` 的子数组有 `1`、`1 4`、`1 4 2`、`1 4 2 3`, 对于每一个子数组的贡献分别都为`1`；
包含 `2` 的子数组有 `2`、`2 3`、`4 2`、`1 4 2`、`1 4 2 3`, 对于每一个子数组的贡献分别为`0, 0, 0, 1, 1`；
包含 `3` 的子数组有 `3`、`2 3`、`4 2 3`、`1 4 2 3`, 对于每一个子数组的贡献分别为`0, 0, 0, 1`；
包含 `4` 的子数组有 `4`、`1 4`、`4 2`、`4 2 3`、`1 4 2`、`1 4 2 3`, 对于每一个子数组的贡献分别都为`0, 0, 0, 0, 0, 1`。
总贡献为 $4 + 2 + 1 + 1 = 8$

所以，我们可以计算，对于每一个 $i(1 \leq i \leq n)$ ，对于答案的贡献是多少。
我们可以用一个数组 `pos` 记录每个数字在原数组中的位置。对于每个 $i$，我们维护当前 $1$ 到 $i$ 的最小和最大下标 $l, r$，那么只要区间 $[l, r]$ 内恰好包含 $1$ 到 $i$，这个区间就是一个合法的不动点区间。对于每个 $i$，以 $[l, r]$ 为区间的所有子数组的数量为 $l \times (n - r + 1)$，累加即可得到答案。

### 代码(c++)：
时间复杂度 **O($n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

void go() {
    int n;
    cin >> n;
    vector<int> a(n + 1, 0), pos(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        pos[a[i]] = i;
    }

    long long ans = 0;
    int l = INT_MAX, r = INT_MIN;
    for (int i = 1; i <= n; i++) {
        l = min(pos[i], l);
        r = max(pos[i], r);
        ans += 1LL * l * (n - r + 1);
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        go();
    }
    return 0;
}
```