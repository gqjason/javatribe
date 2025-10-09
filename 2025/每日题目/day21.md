>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[中位数](https://ac.nowcoder.com/acm/problem/297839)
### 思路：

1.核心目标：通过删除策略保留尽可能大的元素，使最终剩余≤2个元素的中位数（下取整）最大；
2.关键策略：原数组元素依次加入临时数组，当临时数组长度达3时，排序后删除其中位数（优先删较小中间值），始终保留最大和最小元素；
因此，只有最大值和最小值不会被删，剩下的只会是这两个数。答案就是 `(max_num + min_num)/2`

### 代码(c++)：

以下代码时间复杂度为 **O( $nlog n$)**，主要为排序的时间复杂度。(**O(n)** 更快，直接找最大值和最小值。)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(n), t;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    sort(a.begin(), a.end());
    cout << (a[0] + a[n-1]) / 2 << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

# 二、提高题
### 题目：[再编号](https://ac.nowcoder.com/acm/problem/17375)
### 思路：
一道思维数学题。
**先分析样例**
原始数组(第0次再编号): `1 2 3 4`, $sum_0=10$
第1次再编号: `9 8 7 6`, $sum_0=30$
第2次再编号: `21 22 23 24`, $sum_0=90$
第3次再编号: `69 68 67 66`, $sum_0=270$
第4次再编号: `201 202 203 204`, $sum_0=810$
第5次再编号: `609 608 607 606`, $sum_0=2430$

我们可以直观地发现，相邻两项的总和满足递推关系：$sum_i = (n-1) \times sum_{i-1}$ 为等比数列。对于偶数和奇数编号，元素之间分别存在如下关系：

\[
\begin{cases}
a_{t_i} = a_{1_i} + \dfrac{sum_{t} - sum_{1}}{n}, & \text{当 $t$ 为奇数} \\
a_{t_i} = a_{0_i} + \dfrac{sum_{t} - sum_{0}}{n}, & \text{当 $t$ 为偶数}\\
1 \leq i \leq n
\end{cases}
\]

这样，就把公式推出来了
最后将各公式整合起来
\[
\begin{cases}
Q_2 : sum_t = sum_0 \times (n-1)^t \\
Q_1 : a_{t_i} = a_{(t \bmod 2)_i} + \dfrac {sum_{t} - sum_{(t \bmod 2)}} {n} (1 \leq i \leq n)\\
\end{cases}
\]
**预处理**
我们只需计算出`t=0`和`t=1`时的每个元素的值和 $sum_0 \text{与} sum_1$ (当然也可以顺便预处理 $sum_{t(1 \leq t \leq 100000)}$ 这样后面就不用再次计算)

**单次询问处理**
对每个询问`(x, t)`：
我们只需先计算 $Q_1$ (询问时再计算 $Q_1$ 需要用到 ***快速幂*** ), 再计算 $Q_2$


**⚠⚠⚠：**
1. 保险起见，每步运算时需取模
2. $Q_2$ 中除 $n$ 时可能需要用到 ***逆元***

**可能需要用到的知识：**
- 快速幂：
学习网站：[OI Wiki](https://oi-wiki.org/math/binary-exponentiation/) ，B站：[快速幂都能做什么？小小的算法也有大大的梦想【五点七边】](https://www.bilibili.com/video/BV16Z4y1M7y1/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
- 逆元：
学习网站：[OI Wiki](https://oi-wiki.org/math/number-theory/inverse/) ，B站：[算法讲解099【扩展】 逆元和除法同余、容斥原理【左程云】](https://www.bilibili.com/video/BV1SW4y1F7Tq/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
### 代码(c++)：

时间复杂度：预处理 **$O(n)$**，单次询问 **$O(logt)$**（询问时再计算 $Q_1$），总复杂度 **$O(n + m log t)$** 。

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int N = 1e5 + 10;
const int mod = 1e9 + 7;

int n, m;
int tot = 0;
int sum[N];
int a[N];

//快速幂
int qpow(int a, int b) {
    int res = 1;
    while (b) {
        if (b & 1) res = res * a % mod;
        a = a * a % mod;
        b >>= 1;
    }
    return res;
}

void solve() {
    cin >> n >> m;
    tot = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        tot = (tot + a[i]) % mod;
    }

    //预处理
    vector<vector<int>> v(2, vector<int>(n + 1, 0));
    for (int i = 1; i <= n; i++) {
        v[0][i] = a[i] % mod;
        v[1][i] = (tot - a[i] + mod) % mod;
    }

    int invn = qpow(n, mod - 2);//对除数进行除法逆元处理

    while (m--) {
        int x, t;
        cin >> x >> t;

        int power = qpow(n - 1, t);
        int t1 = ((tot * power % mod - tot * qpow(n - 1, t & 1) % mod + mod) % mod) * invn % mod;//计算 Q2

        int res = (t1 + v[t & 1][x]) % mod;
        cout << res << "\n";
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    while (t--) {
        solve();
    }
    return 0;
}
```


