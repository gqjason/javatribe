>刚睡醒的时候，就像刚睡醒的时候一样，有一种刚睡醒的感觉，如果你也是刚睡醒，那么你也刚睡醒

# 每日两题
---


# 一、基础题
### 题目：[小红的不动点分配](https://ac.nowcoder.com/acm/problem/299568)
### 思路：
统计每个数字出现的次数。对于每个 $1 \leq i \leq n$，最多只能分配两个 $i$，即答案为 $\sum_{i=1}^n \min(2, \text{cnt}[i])$。这样可以保证每个位置最多有两个相同的数，且不会超过给定的数量限制。

### 代码(c++)：
时间复杂度 **O($n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;

void solve() {
    int n;
    cin >> n;
    vector<int> cnt(N, 0);
    for (int i = 0; i < 2 * n; ++i) {
        int x;
        cin >> x;
        cnt[x]++;//统计x个数
    }

    int ans = 0;
    for (int i = 1; i <= n; ++i) {
        ans += min(2, cnt[i]);
    }

    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

# 二、提高题
### 题目：[小红的矩阵不动点](https://ac.nowcoder.com/acm/problem/299564)

### 思路：
遍历时，对于 $a_{i,j} = \min(i, j)$ 的情况，`ans+1`即可；对于 $a_{i,j} \neq \min(i, j)$ 的情况，将其用 `pair_count` 储存起来。如果在遍历时发现，存在 $a_{i,j} = \min(k, l)$ 且 $a_{k,l} = \min(i, j)$ ，则说明将两者互换后贡献将加上`2`，这时就可以用`conflict`来表示是否已经遇到这种情况。遍历完后，如果`conflict`为`false`，那么需要遍历`value_count`，判断是否存在 $a_{i,j} = \min(k,l)，(1 \leq i,k \leq n 且 1 \leq j,l \leq m)$ 的情况，如果存在，那么`ans+1`。

### 代码(c++)：
时间复杂度 **O($n \times m$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 505;
int n, m;
vector<vector<int>> a(N, vector<int>(N, 0));
bool conflict = false;
map<pair<int, int>, int> pair_count;
map<int, int> min_count, value_count;
//pair_count记录{a[i][j], min(i,j)}
//min_count记录min(i,j), //value_count记录a[i][j]

void solve() {
    cin >> n >> m;
    int ans = 0;
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            cin >> a[i][j];
            int min_ij = min(i, j);

            //如果a[i][j] != min(i,j)且未发现可以互换使得权值加2的情况
            if (a[i][j] != min_ij && !conflict) {
                pair_count[{a[i][j], min_ij}]++;
                min_count[min_ij]++;
                value_count[a[i][j]]++;

                //如果发现发现可以互换使得权值加2的情况
                if (pair_count[{min_ij, a[i][j]}]) {
                    ans += 2;
                    conflict = true;
                }
            } else if (a[i][j] == min_ij) {
                ans++;
            }
        }
    }

    if (!conflict) {
        for (auto &item : value_count) {
            if (min_count[item.first]) {
                ans++;
                break;
            }
        }
    }

    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```


