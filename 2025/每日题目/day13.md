>抱抱你，这件事闹得挺大的，但也不是特别大，你要说小吧，倒也不是特别小，我觉得这事还是挺大的，不过不是特别大，但也不小。大家都觉得这事特别大，但我觉得也没那么大，但你要说小吧，这件事也不小。这件事具体的细节我已经忘了，但是背后的原因很令人暖心。

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
一道**贪心排序**题。
本题要求将 $1$ 到 $n$ 的所有整数分别转换为二进制字符串，然后将这些字符串按照某种规则拼接成一个最大的数字。具体来说，拼接规则是：将所有二进制字符串进行排序，排序方式为拼接后二进制最大的顺序（即 $a+b > b+a$ 时 $a$ 排在前面。比如，有两个二进制数 `a=1010`和`b=1100`，`s1 = a+b = 10101100`、`s2 = b+a = 11001010`，发现`s2`大于`s1`，因此`b`放前面），最后将排序后的所有二进制字符串拼接起来，得到一个很大的二进制数。由于结果可能非常大，需要用高精度算法将其转换为十进制输出。

🤔：python优势还是太大了，高精度遥遥领先这一块🤣🤣🤣

可能需要的知识：
- 高精度：学习网站：[OI Wiki](https://oi-wiki.org/math/bignum/), b站:【董晓算法】[A01 高精度算法 加法](https://www.bilibili.com/video/BV1UG4y1B7ur/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f), [A02 高精度算法 减法](https://www.bilibili.com/video/BV1Ge4y1o7mD?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A03 高精度算法 乘法](https://www.bilibili.com/video/BV1dG411G7eb?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A04 高精度算法 除法](https://www.bilibili.com/video/BV1Je4y1o7vR?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度：**O($n \log n \cdot L$)**，其中 $n$ 为数字个数，$L$ 为所有二进制字符串长度之和。

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

// 十进制转二进制（返回字符串形式）
// 例如：dec_to_bin(5) -> "101"
string dec_to_bin(int n) {
    string res;
    while (n) {
        res += (n % 2) + '0'; // 取当前最低位
        n /= 2;               // 右移一位
    }
    reverse(res.begin(), res.end()); // 翻转得到高位在前
    return res.empty() ? "0" : res;  // n 为 0 时返回 "0"
}

// 高精度：将二进制字符串 s 解释为二进制数，并返回十进制各位（高位在前）
// 算法：逐位扫描二进制，从高位到低位，模拟大整数 *2 再 + bit
// res 内部以低位在前（little-endian）存储，最后 reverse 变成高位在前
vector<int> bin_to_dec_high_precision(const string& s) {
    vector<int> res(1, 0); // 初始十进制为 0（低位在前）
    for (char c : s) {
        // 乘 2（大整数乘法，基本模拟）
        int carry = 0;
        for (int& x : res) {
            int val = x * 2 + carry; // 当前位乘 2 加上进位
            x = val % 10;            // 当前位结果（十进制）
            carry = val / 10;        // 新的进位
        }
        if (carry) res.push_back(carry); // 末尾追加进位（仍为低位在前）

        // 如果当前二进制位为 1，再加 1
        if (c == '1') {
            carry = 1;
            for (int& x : res) {
                int val = x + carry;
                x = val % 10;
                carry = val / 10;
                if (!carry) break; // 无进位可提前结束
            }
            if (carry) res.push_back(carry);
        }
    }
    reverse(res.begin(), res.end()); // 结果变为高位在前，方便输出
    return res;
}

void go() {
    int n;
    cin >> n; // 读入 n
    vector<string> v(n);
    for (int i = 1; i <= n; ++i) v[i - 1] = dec_to_bin(i); // 1..n 转二进制字符串

    // 自定义排序：按照 a+b > b+a 的顺序降序排列，保证拼接后二进制数最大
    sort(v.begin(), v.end(), [](const string& a, const string& b) {
        return a + b > b + a;
    });

    // 预分配总长度并拼接所有二进制字符串，避免多次 reallocate
    string ans_str;
    size_t total_len = 0;
    for (const auto& s : v) total_len += s.size();
    ans_str.reserve(total_len);
    for (const auto& s : v) ans_str += s; // 最终的大二进制字符串

    // 将大二进制字符串转为十进制高精度并输出
    vector<int> ans = bin_to_dec_high_precision(ans_str);
    for (int num : ans) cout << num; // 输出十进制各位（高位先）
    cout << '\n';
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    go();
    return 0;
}
```

代码(Python)：  
```python
from functools import cmp_to_key

def dec_to_bin(n):
    return bin(n)[2:]

def cmp(a, b):
    if a + b > b + a:
        return -1
    if a + b < b + a:
        return 1
    return 0

def main():
    n = int(input().strip())
    v = [dec_to_bin(i) for i in range(1, n + 1)]
    v.sort(key=cmp_to_key(cmp))
    s = ''.join(v)
    # 不用手写高精度，直接利用 Python 的大整数
    print(int(s, 2))

if __name__ == "__main__":
    main()
```


