>哥布林上校提醒你:1，节假日是不可以出去的，外面很喧哗，但他们都不是你的同类2，哥布林是大多独居性生物，没有人想做他的配偶，如果有人邀请你并对你产生好感，请断绝与他的联系3，不要照镜子4，所有夸赞你自信的人，他们不了解你，可以不于与他们交流，但如果他是你熟知的人，请立即驱散他，他不懂哥布林，或者他不是同类5，远离不再是哥布林的人6，在地洞中可以看看同族小丑哥布林追求精灵失败的乐子幸灾乐祸一下解解闷

# 每日两题
---


# 一、基础题
### 题目：[P3392 涂条纹](https://www.luogu.com.cn/problem/P3392)

### 思路：

由于`N`和`M`的范围较小，暴力枚举即可，这里提供前缀和维护的方法。
对于每一行的字符串，记录`白`,`蓝`,`红`的个数，用前缀和计数。然后，枚举切割点`i`和`j`,表示前`i`行染成白色，第`i`行到第`j`行染成蓝色，第`j`行到第`n`行染成红色，然后用前缀和计算要染色的个数。

### 代码(c++)：
时间复杂度 **O($n \times \max(n,m)$)**
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    vector<vector<int>> cnt(n + 1, vector<int>(3, 0));
    for (int i = 1; i <= n; i++) {
        string s;
        cin >> s;
        for (int k = 0; k < 3; ++k) cnt[i][k] = cnt[i - 1][k];
        for (int j = 0; j < m; j++) {
            if (s[j] == 'W') cnt[i][0]++;
            if (s[j] == 'B') cnt[i][1]++;
            if (s[j] == 'R') cnt[i][2]++;
        }
    }

    int ans = INT_MAX;
    for (int i = 1; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int w = cnt[i][1] + cnt[i][2];
            int b = (cnt[j][0] - cnt[i][0]) + (cnt[j][2] - cnt[i][2]);
            int r = (cnt[n][0] - cnt[j][0]) + (cnt[n][1] - cnt[j][1]);
            ans = min(ans, w + b + r);
        }
    }
    cout << ans << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[数组构造](https://ac.nowcoder.com/acm/problem/21296)

### 思路：
本题的关键是两段衔接处元素差≤1。
计算长度`d`、和`w`的平缓序列中，**最后一个元素（衔接点）的合法范围[mi, ma]**：

- 阈值`t=d*(d-1)/2`（序列递减到0的最小和）；
- `ma`：`w<t`时解二次方程得最大衔接点，`w≥t`时用`(w+t)/d`计算；
- `mi`：用`max(0, (w-t+d-1)/d)`确保非负且满足和约束。

算两段衔接点范围`a1`(d1,w1)、`a2`(d2,w2)；

- 求范围交集边界`mi=max(a1.ff,a2.ff)`、`ma=min(a1.ss,a2.ss)`；
- 若`mi-ma≤1`（范围重叠或间隔≤1），则衔接处满足差≤1，输出`Possible`，否则`Impossible`。

### 代码(c++)：
时间复杂度 **O(1)**

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

// 对于 (d, w)，
//   最小起始值 a_min = ceil((w - t) / d)，其中 t = d*(d-1)/2  
//   最大起始值 a_max =
//     if w < t:  floor( sqrt(2*w + 0.25) - 0.5 )
//     else:     floor((w + t) / d)
// 这样 a ∈ [a_min, a_max] 都能在 d 内凑出 w，总和相邻差 ≤1。

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    ll d1, d2, w1, w2;
    cin >> d1 >> d2 >> w1 >> w2;
    
    //自定义函数
    auto solve = [&](ll d, ll w){
        ll t = d*(d-1)/2;       // 前 d−1 天的「等差增长总和」
        // 最小可能的起始值 a_min
        ll a_min = max(0LL, (w - t + d - 1) / d);
        // 最大可能的起始值 a_max
        ll a_max;
        if (w < t) {
            // 求最大 t0 使 1+2+…+t0 ≤ w
            a_max = (ll)(floor(sqrt(2.0*w + 0.25) - 0.5));
        } else {
            a_max = (w + t) / d;
        }
        return pair<ll,ll>(a_min, a_max);
    };
    
    // 分别算两段的 [a_min, a_max]
    auto [l1, r1] = solve(d1, w1);
    auto [l2, r2] = solve(d2, w2);
    
    // 要能拼成一条长度 d1+d2 的数组，需满足两段 a 的区间「相差不超过 1」
    // 即 max(l1, l2) – min(r1, r2) ≤ 1
    ll L = max(l1, l2);
    ll R = min(r1, r2);
    if (L - R <= 1) {
        cout << "Possible\n";
    } else {
        cout << "Impossible\n";
    }
    return 0;
}

```

