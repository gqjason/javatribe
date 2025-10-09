>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[P1271 【深基9.例1】选举学生会](https://www.luogu.com.cn/problem/P1271)
### 思路：

本题要求对学生会选举的投票结果进行统计，并按照学号从小到大输出每个学生获得的票数。可以使用一个数组来统计每个学号的票数，然后遍历数组，按顺序输出每个学号对应的票数。

### 代码(c语言)：
时间复杂度 **O(m + n)**
```c
#include <stdio.h>
#include <string.h>

int cnt[100010];

int main() {
    int n, m, x;
    scanf("%d%d", &n, &m);
    memset(cnt, 0, sizeof(cnt));
    for (int i = 0; i < m; i++) {
        scanf("%d", &x);
        cnt[x]++;//桶计数
    }
    for (int i = 1; i <= n; i++) {
        while (cnt[i]--) {//当不为0时循环输出
            printf("%d ", i);
        }
    }
    printf("\n");
    return 0;
}
```

# 二、提高题
### 题目：[P12186 [蓝桥杯 2025 省 Python A/研究生组] 最大数字](https://www.luogu.com.cn/problem/P12186)
### 思路：

本题要求将 $1$ 到 $n$ 的所有整数分别转换为二进制字符串，然后将这些字符串按照某种规则拼接成一个最大的数字。具体来说，拼接规则是：将所有二进制字符串进行排序，排序方式为拼接后字典序最大的顺序（即 $a+b > b+a$ 时 $a$ 排在前面），最后将排序后的所有二进制字符串拼接起来，得到一个很大的二进制数。由于结果可能非常大，需要用高精度算法将其转换为十进制输出。

主要步骤如下：
1.枚举 $1$ 到 $n$，将每个数转换为二进制字符串。
2.按照拼接后字典序最大的方式对所有二进制字符串排序。
3.将排序后的所有字符串拼接成一个大二进制数。
4.使用高精度算法将大二进制数转换为十进制并输出。

🤔：python优势还是太大了，高精度遥遥领先这一块🤣🤣🤣

可能需要的知识：
- 高精度：学习网站：[OI Wiki](https://oi-wiki.org/math/bignum/), b站:【董晓算法】[A01 高精度算法 加法](https://www.bilibili.com/video/BV1UG4y1B7ur/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f), [A02 高精度算法 减法](https://www.bilibili.com/video/BV1Ge4y1o7mD?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A03 高精度算法 乘法](https://www.bilibili.com/video/BV1dG411G7eb?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A04 高精度算法 除法](https://www.bilibili.com/video/BV1Je4y1o7vR?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f)
### 代码(c++)：
时间复杂度：**O($n \log n \cdot L$)**，其中 $n$ 为数字个数，$L$ 为所有二进制字符串长度之和。
```c
#include <bits/stdc++.h>
#define int long long
using namespace std;

// 十进制转二进制
string dec_to_bin(int n) {
    string res;
    while (n) {
        res += (n % 2) + '0';
        n /= 2;
    }
    reverse(res.begin(), res.end());
    return res.empty() ? "0" : res;
}

// 高精度二进制转十进制
vector<int> bin_to_dec_high_precision(const string& s) {
    vector<int> res(1, 0);
    for (char c : s) {
        // 乘2
        int carry = 0;
        for (int& x : res) {
            int val = x * 2 + carry;
            x = val % 10;
            carry = val / 10;
        }
        if (carry) res.push_back(carry);
        // 加1
        if (c == '1') {
            carry = 1;
            for (int& x : res) {
                int val = x + carry;
                x = val % 10;
                carry = val / 10;
                if (!carry) break;
            }
            if (carry) res.push_back(carry);
        }
    }
    reverse(res.begin(), res.end());
    return res;
}

void go() {
    int n;
    cin >> n;
    vector<string> v(n);
    for (int i = 1; i <= n; ++i) v[i - 1] = dec_to_bin(i);

    sort(v.begin(), v.end(), [](const string& a, const string& b) {
        return a + b > b + a;
    });

    // 预分配空间
    string ans_str;
    size_t total_len = 0;
    for (const auto& s : v) total_len += s.size();
    ans_str.reserve(total_len);
    for (const auto& s : v) ans_str += s;

    vector<int> ans = bin_to_dec_high_precision(ans_str);
    for (int num : ans) cout << num;
    cout << '\n';
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    go();
    return 0;
}

```

