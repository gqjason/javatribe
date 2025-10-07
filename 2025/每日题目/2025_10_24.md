>有没有10r左右的一千块推荐

# 每日两题
---


# 一、基础题
### 题目：[P1138 第 k 小整数](https://www.luogu.com.cn/problem/P1138)

### 思路：
直接排序后去重再找第k小个元素即可

### 代码(c语言)：
时间复杂度 **O($n \log n$)**, 主要为排序的时间复杂度

```cpp
#include <stdio.h>
#include <stdlib.h>

int cmp(const void *a, const void *b) {
    return (*(int*)a) - (*(int*)b);
}

int main() {
    int n, k;
    scanf("%d %d", &n, &k);
    int *a = (int*)malloc((n + 1) * sizeof(int));//初始化
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
    }

    qsort(a + 1, n, sizeof(int), cmp);//排序

    int idx = 0;
    for (int i = 1; i <= n; i++) {
        if (a[i] != a[i - 1]) {
            idx++;//如果与前一个数不相同则找到下一个数，不同的数的数量idx++
        }
        if (idx == k) {
            printf("%d\n", a[i]);//已经找到了第k小的数
            free(a);
            return 0;
        }
    }
    printf("NO RESULT\n");
    free(a);
    return 0;
}
```


# 二、提高题
### 题目：[小红的双排列权值](https://ac.nowcoder.com/acm/problem/296657)

### 思路：
一道思维贪心题。
假设数组`A`为 `1 2 3 2 3 1 4 5 5 4`，权值和 $Q = f(1)+f(2)+f(2)+f(4)+f(5) = 4+1+1+2+0 = 8$。那怎么交换才能使权值和变大呢？通过枚举发现，当某两个区间不相交的数字区间，前者的右端点和后者的左端点交换，才能使得权值变大，且增加量为交换下标之差的两倍，因此找到最小的右端点以及最大的左端点即可。所以只需判断将$A[r_{min}]$与$A[l_{max}]$互换是否增加权值和`Q`即可。

### 代码(c++)：
时间复杂度 **O($n \log n$)**，主要为排序的时间复杂度。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

using ll = long long;

bool cmp(const vector<ll>& a, const vector<ll>& b) {
    return a[0] < b[0];
}

void solve() {
    ll n;
    cin >> n;
    vector<ll> a(2 * n + 1, 0);
    vector<vector<ll>> ind(n + 1, vector<ll>());
    ind[0] = {0, 0};
    for (ll i = 1; i <= 2 * n; i++) {
        ll x;
        cin >> x;
        a[i] = x;
        ind[x].push_back(i);//储存数字的下标
    }
    sort(ind.begin(), ind.end(), cmp);//按左端点排序

    ll ans = 0;
    ll min_r = LLONG_MAX, max_l = LLONG_MIN;
    for (ll i = 1; i <= n; i++) {
        ll l = ind[i][0];
        ll r = ind[i][1];
        min_r = min(min_r, r);
        max_l = max(max_l, l);
        ans += abs(l - r) - 1;
    }
    ll add = max(0LL, max_l - min_r);
    ans += 2 * add;
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    ll t = 1;
    // cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

