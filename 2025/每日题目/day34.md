>哥布林上校提醒你:1，节假日是不可以出去的，外面很喧哗，但他们都不是你的同类2，哥布林是大多独居性生物，没有人想做他的配偶，如果有人邀请你并对你产生好感，请断绝与他的联系3，不要照镜子4，所有夸赞你自信的人，他们不了解你，可以不于与他们交流，但如果他是你熟知的人，请立即驱散他，他不懂哥布林，或者他不是同类5，远离不再是哥布林的人6，在地洞中可以看看同族小丑哥布林追求精灵失败的乐子幸灾乐祸一下解解闷

# 每日两题
---


# 一、基础题
### 题目：[P8218 【深进1.例1】求区间和](https://www.luogu.com.cn/problem/P8218)

### 思路：
使用前缀和优化区间求和。先读入数组并计算前缀和 `prefix_sum[i]`，表示前 `i` 个数的和。每次查询区间 `[l, r]` 的和时，直接用 `prefix_sum[r] - prefix_sum[l-1]` 得到答案，避免重复计算，查询效率高。

可需要的知识：
- 前缀和差分：学习网站： [OI Wiki](https://oi-wiki.org/basic/prefix-sum/) ，b站：[前缀和及其变种，十道题检验你是不是真的学会了【NotOnlySuccess】](https://www.bilibili.com/video/BV1xKfyY9EkN/?spm_id_from=333.1387.upload.video_card.click&vd_source=933c136d6897dbf20ff125fb1209208f) 。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    vector<long long> prefix_sum(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        cin >> prefix_sum[i];
        prefix_sum[i] += prefix_sum[i - 1];
    }

    int q;
    cin >> q;
    while (q--) {
        int l, r;
        cin >> l >> r;
        cout << prefix_sum[r] - prefix_sum[l - 1] << '\n';
    }

    return 0;
}
```

# 二、提高题
### 题目：[好排列](https://ac.nowcoder.com/acm/problem/298870)

### 思路：
通过枚举可得：
- `n`=1时，合法的排列：`1`;
- `n`=2时，合法的排列：`1 2`,`2 1`;
- `n`=3时，合法的排列：`3 1 2`,`1 2 3`,`3 2 1`,`2 1 3`;
- `n`=4时，合法的排列：`4 3 1 2`,`3 1 2 4`,`4 1 2 3`,`1 2 3 4`,`4 3 2 1`,`3 2 1 4`,`4 2 1 3`,`2 1 3 4`;

经过分析可以发现，满足条件的排列数实际上等价于 $2^{n-1}$。这是因为每个位置（除了第一个）都有两种选择，将`n`放`n-1`的合法排序的左边或右边，类似于将 $n$ 个元素分成若干段，每段内部顺序不变，只需考虑分割点的选择。
接下来套 ***快速幂*** 模板即可。

可能需要的知识：
- 快速幂：学习网站：[OI Wiki](https://oi-wiki.org/math/binary-exponentiation/) ，B站：[快速幂都能做什么？小小的算法也有大大的梦想【五点七边】](https://www.bilibili.com/video/BV16Z4y1M7y1/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O($\log n$)**

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

const int mod = 1e9 + 7;

// 快速幂
int ksm(int a, int b) {
    int res = 1;
    while (b) {
        if (b & 1) res = res * a % mod;
        a = a * a % mod;
        b >>= 1;
    }
    return res;
}

void solve() {
    int n;
    cin >> n;
    cout << ksm(2, n - 1) << '\n';
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) solve();
    return 0;
}
```

