>为什么大家群里说的话都在左边，就我的话在右边啊，好难过，感觉被孤立了。

# 每日两题
---


# 一、基础题
### 题目：[P1002 [NOIP 2002 普及组] 过河卒](https://www.luogu.com.cn/problem/P1002)

### 思路：
一道基础的动态规划题。
预处理：将 (x,y) 以及 (x±1,y±2)、(x±2,y±1) 共 9 个格子（边界内）标记为不可达。
- 动态规划：
    - 定义 dp[i][j] 为到达 (i,j) 的路径数；若该格被禁用则为 0。
    - 初始：若 (0,0) 未被禁用，则 dp[0][0] = 1。
    - 转移：若 (i,j) 未被禁用，则
        - dp[i][j] += dp[i-1][j]（当 i>0）
        - dp[i][j] += dp[i][j-1]（当 j>0）
结果：dp[n][m]。

可能需要的知识：
- dp(动态规划)：
学习网站：[OI Wiki](https://oi-wiki.org/dp/)，B站：[不是我催... 以后大家学动态规划，都得要看这个视频。【NotOnlySuccess】](https://www.bilibili.com/video/BV1m9EUzLEB4/?spm_id_from=333.337.search-card.all.click)

### 代码(c++)：
时间复杂度 **O($n \times m$)**
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    long long n, m, x, y;
    cin >> n >> m >> x >> y;

    vector<vector<bool>> blocked(n + 1, vector<bool>(m + 1, false));

    //标记不可经过的点
    auto mark = [&](long long i, long long j) {
        if (0 <= i && i <= n && 0 <= j && j <= m) blocked[i][j] = true;
    };
    mark(x, y);
    const int dx[8] = {1, 1, 2, 2, -1, -1, -2, -2};
    const int dy[8] = {2, -2, 1, -1, 2, -2, 1, -1};
    for (int k = 0; k < 8; ++k) mark(x + dx[k], y + dy[k]);

    vector<vector<long long>> dp(n + 1, vector<long long>(m + 1, 0));
    if (!blocked[0][0]) dp[0][0] = 1;

    //dp递推
    for (long long i = 0; i <= n; ++i) {
        for (long long j = 0; j <= m; ++j) {
            if (blocked[i][j]) continue;
            if (i > 0) dp[i][j] += dp[i - 1][j];
            if (j > 0) dp[i][j] += dp[i][j - 1];
        }
    }

    cout << dp[n][m] << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[P1908 逆序对](https://www.luogu.com.cn/problem/P1908)

### 思路：
一道归并排序的模板题，当然也可以用其他数据结构做。
简单的暴力求逆序对方法会超时，因为`n`范围为 $1 \leq n \leq 5e5$，所以考虑时间复杂度为 O($n\log n$) 的做法。
- 定义：若 i < j 且 a[i] > a[j]，则称 (i, j) 为一对逆序对。
- 做法（归并计数）：
    - 分治：递归将区间 [l, r] 拆成 [l, mid] 与 [mid+1, r]，分别统计左右区间内部的逆序对数。
    - 跨区统计：归并两个有序段时，若左段元素 a[i] > 右段元素 a[j]，则左段区间 [i, mid] 的所有元素都大于 a[j]，贡献 mid - i + 1 个逆序对；否则将较小者放入临时数组。
    - 归并完成后将临时数组拷回原数组，保证每一层递归后区间有序。

也可以用树状数组/线段树 + 离散化统计。将值域压缩后，按从左到右（或从右到左）遍历，查询已出现元素中大于（或小于）当前值的数量并累加，复杂度同为 O(n log n)。
以下提供归并分治方法的代码。

- 可能需要的知识：
归并排序：学习网站：[OI Wiki](https://oi-wiki.org/basic/merge-sort/)，B站：[【排序算法精华2】归并排序【五点七边】](https://www.bilibili.com/video/BV1Ma4y1n7E2/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
线段树：学习网站：[OI Wiki](https://oi-wiki.org/ds/seg/)，B站：[线段树【_justTrying】](https://www.bilibili.com/video/BV1vhPoeXEFf/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
树状数组：学习网站：[OI Wiki](https://oi-wiki.org/ds/fenwick/)，B站：[【C++】七分三十秒，树状数组（洛谷P3374）_动画演示_思路分析_代码实现【诸葛喵】](https://www.bilibili.com/video/BV14tCUYcEDo/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
离散化：学习网站：[OI Wiki](https://oi-wiki.org/misc/discrete/)，b站：[算法讲解048【必备】二维前缀和、二维差分、离散化技巧【左程云】](https://www.bilibili.com/video/BV1Wz4y1K74C/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O($n\log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

//归并排序模板
i64 merge_count(vector<i64>& a, vector<i64>& tmp, int l, int r) {
    if (l >= r) return 0;
    int mid = l + (r - l) / 2;
    i64 inv = 0;
    inv += merge_count(a, tmp, l, mid);
    inv += merge_count(a, tmp, mid + 1, r);

    int i = l, j = mid + 1, k = l;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else {
            tmp[k++] = a[j++];
            inv += mid - i + 1;
        }
    }
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (int p = l; p <= r; ++p) a[p] = tmp[p];

    return inv;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    if (!(cin >> n)) return 0;
    vector<i64> a(n), tmp(n);
    for (int i = 0; i < n; ++i) cin >> a[i];

    cout << merge_count(a, tmp, 0, n - 1) << '\n';
    return 0;
}
```

