>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[P1094 [NOIP 2007 普及组] 纪念品分组](https://www.luogu.com.cn/problem/P1094)

### 思路：

排序 + 双指针贪心：
尽量让最重的与当前能配对的最轻的组合，才能为后面留下更多空间；若最轻的都配不了，则最重的只能单独用。

可能需要的知识：

- 双指针：学习网站：[OI Wiki](https://oi-wiki.org/misc/two-pointer/)，B站：[算法讲解050【必备】双指针技巧与相关题目【左程云】](https://www.bilibili.com/video/BV1V841167Rg/?spm_id_from=333.337.search-card.all.click)

### 代码(c语言)

时间复杂度：排序 **O(n log n)**，双指针 **O(n)**，总 **O(n log n)**。

```cpp
#include <stdio.h>
#include <stdlib.h>

int cmp(const void *a, const void *b) {
    return *(const int*)a - *(const int*)b; // 升序排序
}

int main() {
    int w, n;
    if (scanf("%d %d", &w, &n) != 2) return 0; // 读入最大重量和人数
    int *a = (int*)malloc(sizeof(int) * n);
    if (!a) return 0;
    for (int i = 0; i < n; i++) scanf("%d", &a[i]); // 读入每个人的重量
    qsort(a, n, sizeof(int), cmp); // 排序
    int l = 0, r = n - 1, ans = 0;
    while (l <= r) {
        if (a[l] + a[r] <= w) { // 最轻和最重能配对
            l++; r--;
        } else { // 不能配对，最重的单独分组
            r--;
        }
        ans++; // 分组数+1
    }
    printf("%d\n", ans); // 输出分组数
    free(a);
    return 0;
}

```

# 二、提高题
### 题目：[P1324 矩形分割](https://www.luogu.com.cn/problem/P1324)
### 思路：
**贪心核心**：某条线的原始费用越大，就越希望它在“乘数”还小的时候被执行。执行某一方向的一刀，会使该方向的段数 +1，从而让之后另一方向的切割代价乘数变大。于是策略为把所有横向费用与纵向费用分别降序排序，然后用双指针（或归并思想）每次取当前尚未使用的费用较大的那一条切掉：
1.若下一条最大横向费用 ≥ 最大纵向费用：优先执行该横向切，贡献 = 该费用 $\times$ 当前纵向段数 cnt_v，随后横向段数 cnt_h++。
2.否则执行纵向切，贡献 = 该费用 $\times$ 当前横向段数 cnt_h，随后纵向段数 cnt_v++。
3.一类耗尽后，另一类剩余费用依次乘以最终的对向段数即可。
正确性证明（贪心交换）：假设有两条费用 C1 ≥ C2，分别被安排在较晚和较早的位置，其乘数分别为 $k_{large} > k_{small}$，则当前总花费为 $C1 \times k_{large} + C2 \times k_{small}$。若交换顺序，则变为 $C1 \times k_{small} + C2 \times k_{large}$，差值为 $(C1-C2) \times (k_{large} - k_{small}) ≥ 0$，说明让更大的费用获得更小乘数不会更差，因此贪心策略是最优的。

⚠⚠⚠：使用 long long 防止大数溢出。

可能需要的知识：

- 双指针：学习网站：[OI Wiki](https://oi-wiki.org/misc/two-pointer/)，B站：[算法讲解050【必备】双指针技巧与相关题目【左程云】](https://www.bilibili.com/video/BV1V841167Rg/?spm_id_from=333.337.search-card.all.click)

### 代码(c++)：
时间复杂度：排序 **O($(n+m) \times \log(n+m)$)**，合并/扫描 **O(n+m)**，总 **O((n+m) log(n+m))**。

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n, m;
    cin >> n >> m;
    vector<long long> a(n - 1), b(m - 1);
    for (int i = 0; i < n - 1; ++i) cin >> a[i]; // 横向切割费用
    for (int i = 0; i < m - 1; ++i) cin >> b[i]; // 纵向切割费用

    sort(a.rbegin(), a.rend()); // 横向费用降序排序
    sort(b.rbegin(), b.rend()); // 纵向费用降序排序

    long long ans = 0;
    int i = 0, j = 0, cnt_h = 1, cnt_v = 1; // cnt_h: 当前横向段数, cnt_v: 当前纵向段数
    // 主循环：每次选择当前最大费用的切割
    while (i < n - 1 && j < m - 1) {
        if (a[i] >= b[j]) {
            ans += a[i] * cnt_v; // 横向切割，贡献 = 费用 × 当前纵向段数
            ++i;
            ++cnt_h; // 横向段数+1
        } else {
            ans += b[j] * cnt_h; // 纵向切割，贡献 = 费用 × 当前横向段数
            ++j;
            ++cnt_v; // 纵向段数+1
        }
    }
    // 剩余横向切割
    while (i < n - 1) {
        ans += a[i] * cnt_v;
        ++i;
    }
    // 剩余纵向切割
    while (j < m - 1) {
        ans += b[j] * cnt_h;
        ++j;
    }
    cout << ans << '\n'; // 输出总费用
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

