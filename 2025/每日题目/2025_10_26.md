>我一直觉得搞抽象的都很聪明 和纯粹的发癫不一样 快速反应接梗需要大量的知识储备 还要恰到好处的话术 玩抽象的简直都是天才

# 每日两题
---


# 一、基础题
### 题目：[P12176 [蓝桥杯 2025 省 Python B] 书架还原](https://www.luogu.com.cn/problem/P12176)

### 思路：

看得出来蓝桥杯很喜欢出这样的题，几乎一模一样的题目 [P8637 [蓝桥杯 2016 省 B] 交换瓶子](https://www.luogu.com.cn/problem/P8637#ide)，甚至把本题的代码交上去都能AC 🤣🤣🤣。
在遍历时，若 $a_i \neq i$，一定要交换 $a_i \Leftrightarrow a_{a_i}$ 。遍历一次后就能使得 $a_i = i, 1 \leq i \leq n$。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    if (!(cin >> n)) return 0;

    vector<int> a(n + 1), pos(n + 1);
    for (int i = 1; i <= n; ++i) {
        cin >> a[i];
        pos[a[i]] = i; // 值 -> 位置 的映射
    }

    long long ans = 0;
    for (int i = 1; i <= n; ++i) {
        if (a[i] != i) {
            int j = pos[i];     // 值 i 当前所在的位置
            int v = a[i];       // 位置 i 上的值
            swap(a[i], a[j]);
            pos[v] = j;         // 更新被移动值 v 的位置
            pos[i] = i;         // 值 i 现在位于位置 i
            ++ans;
        }
    }

    cout << ans << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[P1459 [IOI 1996 / USACO2.1] 三值的排序 Sorting a Three-Valued Sequence](https://www.luogu.com.cn/problem/P1459)

### 思路：

这题真的是橙题吗，感觉像黄题🤣。
目标顺序为 1…1, 2…2, 3…3。先统计每种值的数量，确定三个目标段的边界。
构建 3×3 计数矩阵 mis[x][y]：表示“值为 x 却落在应属于 y 段”的数量。
先做成对互换，依次处理 (1,2)、(1,3)、(2,3) 三对：每对可交换 min(mis[x][y], mis[y][x]) 次，加入答案并从矩阵中扣除。
剩余错位必然形成 3 环（1→2→3 或 1→3→2），每个 3 环需要 2 次交换。任取一行的剩余和即为 3 环个数，如 mis[1][2]+mis[1][3]，答案再加其两倍。

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;
    vector<int>a(n);
    vector<int>cnt(4,0);
    for (int i=0;i<n;i++){
        cin >> a[i];
        cnt[a[i]]++;
    }

    int seg1 = cnt[1];
    int seg2 = cnt[1]+cnt[2];

    int mis[4][4] = {};
    for (int i=0;i<n;i++){
        int correct;
        if (i<seg1) correct=1;
        else if (i<seg2) correct=2;
        else correct=3;
        mis[a[i]][correct]++;
    }

    int ans=0;

    int x = min(mis[1][2], mis[2][1]);
    ans += x; mis[1][2]-=x; mis[2][1]-=x;
    x = min(mis[1][3], mis[3][1]);
    ans += x; mis[1][3]-=x; mis[3][1]-=x;
    x = min(mis[2][3], mis[3][2]);
    ans += x; mis[2][3]-=x; mis[3][2]-=x;

    int remain = mis[1][2]+mis[1][3];
    ans += remain*2;

    cout << ans << "\n";
}

```

