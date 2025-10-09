>郑重声明：我不是单身主义，也不是不谈恋爱，我只是纯粹没有人喜欢我，我还泡不到别人

# 每日两题
---


# 一、基础题
### 题目：[P1068 [NOIP 2009 普及组] 分数线划定](https://www.luogu.com.cn/problem/P1068)

### 思路：
按“分数降序、编号升序”排序，保证同分按编号小者在前。
算 k = floor(1.5 × m)（可用 k = 3*m/2 实现），取第 k 名的分数为分数线 `cutoff`。
为处理分数线处的并列，继续向后统计所有分数 ≥ `cutoff` 的考生总数 cnt。
输出 `cutoff` 与 `cnt`，并按排序后的前 `cnt` 名依次输出编号与分数。

### 代码(c++)：
时间复杂度 **O($n\log n$)**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

struct Candidate {
    long long id;
    int score;
};

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);

    int n, m;
    if (!(std::cin >> n >> m)) return 0;

    std::vector<Candidate> a(n);
    for (int i = 0; i < n; ++i) {
        std::cin >> a[i].id >> a[i].score;
    }

    std::sort(a.begin(), a.end(), [](const Candidate& x, const Candidate& y) {
        if (x.score != y.score) return x.score > y.score;
        return x.id < y.id;
    });

    int k = (3 * m) / 2;                 // floor(1.5 * m)
    int cutoff = a[k - 1].score;
    int cnt = 0;
    while (cnt < n && a[cnt].score >= cutoff) ++cnt;

    std::cout << cutoff << ' ' << cnt << '\n';
    for (int i = 0; i < cnt; ++i) {
        std::cout << a[i].id << ' ' << a[i].score << '\n';
    }
    return 0;
}
```

# 二、提高题
### 题目：[氧气少年的 LCM](https://ac.nowcoder.com/acm/problem/266905)

### 思路：

令`a`为 $\gcd(x,y)$，`b`为$\operatorname{lcm}(x,y)$。显而易见的是，`b`是`a`的倍数（因为`x`和`y`是`a`的倍数，而`b`是`x`和`y`的倍数）。
令 d = b/a。由“龟速乘”思想，可将 d 按二进制展开：$d = ∑ 2^{k_j}$，故
$b = a × d = a × (2^{k_1} + 2^{k_2} + … + 2^{k_s}) = ∑ a × 2^{k_j}$。
因此，先用操作1得到 a；再用操作2从 a 出发依次构造 $a × 2^i（1 ≤ i ≤ 64 且 a × 2^i ≤ 10^{18}）$；最后按 d 的二进制为 1 的各位 j，把对应的 $a × 2^j$ 用操作2累加，得到 b。
**举个例子**
取 `x` = 12，`y` = 18：
- `a` = gcd(12, 18) = 6
- `b` = lcm(12, 18) = 36
- `d` = `b/a` = 6，其二进制为 $110_2 = 2^2 + 2^1$
- 用操作2从 a 构造 $a×2^1 = 12、a×2^2 = 24$
- 再按 `d` 的二进制位为 `1` 的项，用操作2把 `24` 与 `12` 相加，得到 `36`，即 `b`。

可能需要的知识：
- 快速幂&龟速乘：学习网站：[(洛谷)快速幂&龟速乘&快速乘](https://www.luogu.com/article/rdvf1eo7)，B站：[快速幂都能做什么？小小的算法也有大大的梦想【五点七边】](https://www.bilibili.com/video/BV16Z4y1M7y1/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O($\log{\max(x,y)}+t$)**，其中t为操作次数。

```cpp
#include <bits/stdc++.h>
using namespace std;

using i64 = long long;

// 题设常量：中间过程与最终值均不应超过 1e18
static const i64 LIM  = 500000000000000000LL;   // 5e17，预构造上限
static const i64 MAXV = 1000000000000000000LL;  // 1e18，题面最大值

// 记录操作序列：op = {type, u, v}
// type = 1 -> 题意中的“操作1”
// type = 2 -> 题意中的“操作2”（表示把 u 与 v 相加，得到 u+v）
vector<array<i64, 3>> ops;

// 功能：已知 b = a * d（d >= 1），将 d 的二进制展开为若干 2^k 之和，
//      再用“操作2”把对应的 a*2^k 依次累加，构造出 b。
// 约束：只在 d 含有至少两个 1 位时才需要做显式累加；否则由倍增即可得到。
static void build_by_bits(i64 d, i64 a) {
    if (__builtin_popcountll((unsigned long long)d) <= 1) return; // 单项无需显式求和

    // 预收集需要的项：a*2^k（仅对 d 的置位 k）
    vector<i64> terms;
    i64 v = a;
    for (int k = 0; k < 63 && v <= MAXV; ++k) {
        if (d & (1ULL << k)) terms.push_back(v);
        if (v > MAXV / 2) break;  // 下一次左移会超过 1e18
        v <<= 1;                  // v = a * 2^(k+1)
    }

    // 逐项用操作2累加：(((t0 + t1) + t2) + ...)
    i64 cur = terms[0];
    for (size_t i = 1; i < terms.size(); ++i) {
        ops.push_back({2, cur, terms[i]});
        cur += terms[i];
    }
}

static void solve_one() {
    i64 x, y;
    if (!(cin >> x >> y)) return;

    ops.clear();

    // a = gcd(x,y), b = lcm(x,y)
    i64 g = std::gcd(x, y);
    __int128 lcm128 = (__int128)x / g * y; // 用 128 位避免中间溢出
    i64 lcm = (i64)lcm128;

    // 若 lcm 等于任一输入，按题意无需操作
    if (lcm == x || lcm == y) {
        cout << 0 << '\n';
        return;
    }

    // 先做两次“操作1”（按题意：用于得到 a = gcd(x,y)，并留足后续需要的副本）
    ops.push_back({1, x, y});
    ops.push_back({1, x, y});

    // 预先用“操作2”从 a 出发构造 a, 2a, 4a, 8a, ...
    // 每个幂次做两次：
    //  - 一次用于产生下一次倍增所需的“种子”
    //  - 一次用于在最终按位求和阶段作为可用的 a*2^k
    for (i64 v = g; v <= LIM; ) {
        ops.push_back({2, v, v}); // v + v -> 2v
        ops.push_back({2, v, v}); // 再做一次，留副本以便后续求和
        if (v > LLONG_MAX / 2) break; // 防止移位 UB
        v <<= 1;
    }

    // d = lcm / a，根据 d 的二进制把 a*2^k 累加得到 b
    i64 d = lcm / g;
    build_by_bits(d, g);

    // 输出操作序列
    cout << ops.size() << '\n';
    for (auto &op : ops) {
        cout << op[0] << ' ' << op[1] << ' ' << op[2] << '\n';
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T = 1;
    cin >> T;
    while (T--) solve_one();
    return 0;
}
```

