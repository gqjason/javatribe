>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[P1051 [NOIP 2005 提高组] 谁拿了最多奖学金](https://www.luogu.com.cn/problem/P1051)
### 思路：
简单题简单做。
没什么好说的，一道代码量不大的模拟题，依照题目要求判断即可。
### 代码(c语言)：
时间复杂度 **O(n)**
```c
#include <stdio.h>
#include <string.h>

struct Node {
    char name[25];
    int ave;
    int dis;
    char ismaster;
    char iswest;
    int let;
    int sum;
};

const int N = 110;
struct Node a[N];

int main(void) {
    int n;
    if (scanf("%d", &n) != 1) return 0;
    for (int i = 0; i < n; i++) {
        scanf("%s %d %d %c %c %d",a[i].name, &a[i].ave, &a[i].dis, &a[i].ismaster, &a[i].iswest, &a[i].let);
        a[i].sum = 0;
        if (a[i].ave > 80 && a[i].let >= 1) a[i].sum += 8000;
        if (a[i].ave > 85 && a[i].dis > 80) a[i].sum += 4000;
        if (a[i].ave > 90) a[i].sum += 2000;
        if (a[i].ave > 85 && a[i].iswest == 'Y') a[i].sum += 1000;
        if (a[i].dis > 80 && a[i].ismaster == 'Y') a[i].sum += 850;
    }

    int mx = -1;
    char best[25] = "";
    int tot = 0;
    for (int i = 0; i < n; i++) {
        if (a[i].sum > mx) {
            mx = a[i].sum;
            strcpy(best, a[i].name);
        }
        tot += a[i].sum;
    }

    printf("%s\n%d\n%d\n", best, mx, tot);
    return 0;
}
```

# 二、提高题
### 题目：[P1249 最大乘积](https://www.luogu.com.cn/problem/P1249)
### 思路：
小学二年级时我们就知道，直角四边形周长一定时，长宽相等时四边形面积最大。所以，n分解为互不相同的连续自然数（从2开始）时乘积最大【避免出现1(1不增大乘积)】。
若连续自然数和小于目标数，将剩余值从大到小均匀分配给已选数（如和为9、目标10，剩余1加给最大数 4→5，得 2+3+5）。
最后，用大数乘法（避免溢出）计算乘积，按要求输出方案（升序）和乘积。
特判：n≤4时直接输出自身（3→3，4→4，乘积已最大）。

可能需要的知识：

- 高精度：
学习网站：[OI Wiki](https://oi-wiki.org/math/bignum/), b站:【董晓算法】[A01 高精度算法 加法](https://www.bilibili.com/video/BV1UG4y1B7ur/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f), [A02 高精度算法 减法](https://www.bilibili.com/video/BV1Ge4y1o7mD?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A03 高精度算法 乘法](https://www.bilibili.com/video/BV1dG411G7eb?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f), [A04 高精度算法 除法](https://www.bilibili.com/video/BV1Je4y1o7vR?spm_id_from=333.788.videopod.sections&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

// 1. 生成最优分解方案（互不相同自然数，乘积最大）
vector<int> get_decomposition(int n) {
    vector<int> res;
    if (n <= 4) {  // 小数字特殊处理：1→1，2→2，3→3，4→4（或2+2，乘积相同）
        res.push_back(n);
        return res;
    }
    int start = 2, sum = 0;
    // 累加从2开始的连续自然数，直到和超过n
    while (sum + start <= n) {
        res.push_back(start);
        sum += start;
        start++;
    }
    // 处理剩余值，均匀分配到末尾（避免重复）
    int remain = n - sum;
    for (int i = res.size() - 1; i >= 0 && remain > 0; i--) {
        res[i]++;
        remain--;
    }
    // 若仍有剩余（仅remain=1，需加到最后一个数，避免出现1）
    if (remain == 1) {
        res.back()++;
    }
    return res;
}

// 2. 大数乘法（逆序存储每一位，避免溢出）
vector<int> calc_product(const vector<int>& nums) {
    vector<int> prod;
    prod.push_back(1);  // 初始乘积为1（逆序：prod[0]是个位）
    for (int num : nums) {
        int carry = 0;
        // 遍历当前乘积的每一位，与num相乘
        for (int i = 0; i < prod.size(); i++) {
            int temp = prod[i] * num + carry;
            prod[i] = temp % 10;
            carry = temp / 10;
        }
        // 处理剩余进位
        while (carry > 0) {
            prod.push_back(carry % 10);
            carry /= 10;
        }
    }
    return prod;
}

// 3. 主逻辑函数
void solve() {
    int n;
    cin >> n;
    vector<int> decomposition = get_decomposition(n);
    vector<int> product = calc_product(decomposition);

    // 输出分解方案（已按升序）
    for (int i = 0; i < decomposition.size(); i++) {
        if (i > 0) cout << " ";
        cout << decomposition[i];
    }
    cout << endl;

    // 输出乘积（逆序存储，需反转后去前导0）
    reverse(product.begin(), product.end());
    bool skip_zero = true;
    for (int bit : product) {
        if (bit != 0) skip_zero = false;
        if (!skip_zero) cout << bit;
    }
    cout << endl;
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;  // 如需多组测试，取消注释
    while (t--) {
        solve();
    }
    return 0;
}
```


