>我一直觉得搞抽象的都很聪明 和纯粹的发癫不一样 快速反应接梗需要大量的知识储备 还要恰到好处的话术 玩抽象的简直都是天才

# 每日两题
---


# 一、基础题
### 题目：[P1125 [NOIP 2008 提高组] 笨小猴](https://www.luogu.com.cn/problem/P1125)

### 思路：
记录字符串`s`中每个字母进行计数，找到`maxn`和`minn`，判断`maxn-minn`是否为质数即可。

### 代码(c++)：
时间复杂度 **O($len(s)$)**, 其中s为输入的字符串

```cpp
#include <bits/stdc++.h>
using namespace std;

//判断是否为质数
bool is_prime(int n) {
    if (n <= 1) return false;
    if (n == 2) return true;
    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) return false;
    }
    return true;
}

void solve() {
    string s;
    cin >> s;
    int cnt[26] = {0};
    for (char c : s) {
        cnt[c - 'a']++;
    }

    int mx = 0, mi = INT_MAX;
    for (int i = 0; i < 26; ++i) {
        if (cnt[i]) {
            mx = max(mx, cnt[i]);
            mi = min(mi, cnt[i]);
        }
    }

    int diff = mx - mi;
    if (is_prime(diff)) {
        cout << "Lucky Word\n" << diff << '\n';
    } else {
        cout << "No Answer\n0\n";
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

# 二、提高题
### 题目：[P1326 足球](https://www.luogu.com.cn/problem/P1326)

### 思路：
纯纯贪心题。最大积分时尽量多胜，最小积分时尽量多负或平。通过枚举剩余比赛的胜负情况，结合已知数据，分别计算最大和最小可能积分。
### 代码(c++)：
时间复杂度 **O($n$)**，其中`n`为输入多少组数据

```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;

// s: 已经赢的场数
// t: 已经平的场数
// n: 总场数

// 计算最小分数
int calc_min(int s, int t, int n) {
    // 如果进球多于丢球，则最小分数为3分加上剩余比赛全输
    if (s > t) return 3 + max(0LL, n - t - 1);
    // 否则，最小分数为3分加上剩余比赛全输或全平，取较小值
    return min(3 + max(0LL, n - t - 1), max(n - t + s, 0LL));
}

// 计算最大分数
int calc_max(int s, int t, int n) {
    if (s < n) {
        // 如果还有进球数小于比赛数，则最大分数为赢局*3加上(剩余比赛-1)全和
        int res = 3 * s + n - s - 1;
        if (!t) res++; // 如果没有丢球，多平一局
        return res;
    } else {
        // 否则，最大分数为3*max(赢n-1局，min(全赢，s-(n-1)>t))
        int res = 3 * max(n - 1, min(n, s - t));
        if (n - s + t == 1) res++; // s-(n-1)=t即得到一局平局
        return res;
    }
}

void solve() {
    int g, ng, cps;
    // 多组输入，每组分别计算最大最小分数
    while (cin >> g >> ng >> cps) {
        int ans_max = calc_max(g, ng, cps);
        int ans_min = calc_min(g, ng, cps);
        cout << ans_max << ' ' << ans_min << endl;
    }
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```


