>活着太累了 开始预售我葬礼的门票 出钱多的可以坐我棺材板上 榜一可以和我一起合葬

# 每日两题
---


# 一、基础题
### 题目：[token](https://ac.nowcoder.com/acm/problem/293548)

### 思路：
两种方法：
1. 滑动窗口：
    - 维护当前窗口和 sum，先累加前 min(10, n) 个元素作为初值。
    - 依次右移窗口：加入 a[i]，若窗口超过 10，则减去 a[i-10]。
2. 前缀和：
    - 构建 ps，ps[0]=0，ps[i]=ps[i-1]+a[i]。
    - 枚举 i∈[1..n]，答案为 max(ps[i] - ps[max(0, i-10)])。
    - 代码中通过把原数组整体右移 10 位，相当于在前面补 10 个 0，从而可直接用 ps[i]-ps[i-10] 计算。

**⚠⚠⚠**：记得开long long

可能需要的知识：
- 前缀和差分：学习网站： [OI Wiki](https://oi-wiki.org/basic/prefix-sum/) ，b站：[前缀和及其变种，十道题检验你是不是真的学会了【NotOnlySuccess】](https://www.bilibili.com/video/BV1xKfyY9EkN/?spm_id_from=333.1387.upload.video_card.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O(n)**，以下提供前缀和的代码。
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    if (!(cin >> n)) return 0;

    // 前置 10 个 0，统一用长度为 10 的窗口
    vector<long long> a(n + 11, 0);
    for (int i = 1; i <= n; ++i) cin >> a[i + 10];

    // 前缀和
    for (int i = 1; i <= n + 10; ++i) a[i] += a[i - 1];

    long long ans = 0; // 若可能有负数且需要精确，可改为 LLONG_MIN
    for (int i = 10; i <= n + 10; ++i) {
        ans = max(ans, a[i] - a[i - 10]);
    }

    cout << ans << '\n';
    return 0;
}
```
### 题目：[B3755 [信息与未来 2019] 方格覆盖](https://www.luogu.com.cn/problem/B3755)

### 思路：
一道思维贪心题。
- 扫描放置（构造法）：按行从左到右、从上到下扫描：
    - 若当前格子未覆盖，优先尝试与右侧格子配对成水平多米诺；
    - 若右侧不可行（越界/禁用/已占用），再尝试与下侧配对成垂直多米诺；
    - 成功放置则为两格赋同一编号 cnt，并自增。
- 正确性要点：贪心的“先右后下”行优先扫描不会产生重叠；每个未覆盖格子在首次被访问时即被就地配对，存在解时可构造出一种合法覆盖。必要条件是可覆盖格子数为偶数（n^2−k 为偶）。


### 代码(c++)：
时间复杂度 **O($n^2$)**
```cpp
#include <bits/stdc++.h>
using namespace std;


int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, k;
    if (!(cin >> n >> k)) return 0;

    // 使用 1-based；0 = 空/未覆盖，-1 = 禁用，>0 = 多米诺编号
    vector<vector<int>> a(n + 1, vector<int>(n + 1, 0));

    // 将主对角线上的前 k 个格子标记为禁用
    for (int i = 1; i <= n && i <= k; ++i) a[i][i] = -1;

    int id = 1; // 当前可用的多米诺编号
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (a[i][j] == 0) {
                // 优先尝试水平放置（与右侧配对）
                if (j + 1 <= n && a[i][j + 1] == 0) {
                        a[i][j] = a[i][j + 1] = id++;
                }
                // 否则尝试垂直放置（与下侧配对）
                else if (i + 1 <= n && a[i + 1][j] == 0) {
                        a[i][j] = a[i + 1][j] = id++;
                }
                // 若两种方式都不行，则该格保持未覆盖（输出时按 0 处理）
            }
        }
    }

    // 输出：禁用格与未覆盖格输出 0，其余输出多米诺编号
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            cout << (a[i][j] < 0 ? 0 : a[i][j]) << (j == n ? '\n' : ' ');
        }
    }
    return 0;
}
```

