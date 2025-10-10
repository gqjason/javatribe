>打开这条帖子的人，三天之内脱单，不灵回来找我，我给你一锤，不争气的东西

# 每日两题
---


# 一、基础题
### 题目：[a^b](https://ac.nowcoder.com/acm/problem/50906)

### 思路：
***快速幂*** 模板题。

可能需要的知识：
- 快速幂：学习网站：[OI Wiki](https://oi-wiki.org/math/binary-exponentiation/) ，B站：[快速幂都能做什么？小小的算法也有大大的梦想【五点七边】](https://www.bilibili.com/video/BV16Z4y1M7y1/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O($\log b$)**
```cpp
#include <bits/stdc++.h>
using namespace std;

long long mod_pow(long long a, long long b, long long m) {
    if (m == 1) return 0;
    a %= m;
    long long res = 1 % m;
    while (b > 0) {
        if (b & 1) res = ( (__int128)res * a ) % m;
        a = ( (__int128)a * a ) % m;
        b >>= 1;
    }
    return res;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    long long a, b, p;
    if (!(cin >> a >> b >> p)) return 0;
    cout << mod_pow(a, b, p) << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[P1631 序列合并](https://www.luogu.com.cn/problem/P1631)

### 思路：
将 A、B 升序排序。
把每个 i 的序列 Li: A[i] + B[0..n-1] 视为已排序链表，需要合并这 n 个链表并取前 n 小和。
用小根堆维护候选三元组(sum, i, j)。初始将 (A[i]+B[0], i, 0) 全部入堆。
重复 n 次：弹出堆顶作为当前最小和；若 j+1 < n，则将 (A[i]+B[j+1], i, j+1) 入堆。
等价于 K 路归并，始终只扩展产生最小和的那条链表的下一个元素。

可能需要的知识：
- 优先队列：学习网站 [(csdn)【原创】优先队列 priority_queue 详解](https://blog.csdn.net/c20182030/article/details/70757660)，b站: [【从堆的定义到优先队列、堆排序】 10分钟看懂必考的数据结构——堆【工程部老周】](https://www.bilibili.com/video/BV1AF411G7cA/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
- 二分：学习网站 [OI Wiki](https://oi-wiki.org/basic/binary/)，b站: [蛋从几层楼往下丢才会碎？一个视频讲明白【NotOnlySuccess】](https://www.bilibili.com/video/BV1DzXzYPEYT/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f) 

### 代码(c++)：
时间复杂度 **O($n\log n$)**

```cpp
#include <iostream>   // 标准输入输出
#include <vector>     // 动态数组
#include <queue>      // 优先队列（堆）
#include <tuple>      // 元组，用于存 (sum, i, j)
#include <algorithm>  // sort
using namespace std;

int main() {
    ios::sync_with_stdio(false); // 关闭与 stdio 的同步，提高 IO 性能
    cin.tie(nullptr);            // 解除 cin 与 cout 绑定

    int n;
    if (!(cin >> n)) return 0;   // 读入 n，失败则退出

    vector<long long> A(n), B(n);
    for (int i = 0; i < n; ++i) cin >> A[i]; // 读入数组 A
    for (int i = 0; i < n; ++i) cin >> B[i]; // 读入数组 B

    sort(A.begin(), A.end()); // 将 A 升序排序
    sort(B.begin(), B.end()); // 将 B 升序排序

    // 小根堆节点：(当前和 sum，下标 i，对应 B 的下标 j)
    using Node = tuple<long long, int, int>;
    priority_queue<Node, vector<Node>, greater<Node>> pq; // 小根堆

    // 初始化：每个 A[i] 与 B[0] 组成最小的一项入堆
    for (int i = 0; i < n; ++i) {
        pq.emplace(A[i] + B[0], i, 0);
    }

    vector<long long> ans;
    ans.reserve(n); // 只需要前 n 小的和

    // 进行 n 次弹出，每次取当前最小的 sum
    for (int k = 0; k < n; ++k) {
        auto [s, i, j] = pq.top(); // 取出最小和对应的三元组
        pq.pop();
        ans.push_back(s);          // 记录答案
        if (j + 1 < n) {           // 扩展到链表的下一个元素：A[i] + B[j+1]
            pq.emplace(A[i] + B[j + 1], i, j + 1);
        }
    }

    // 按要求输出前 n 小的和，空格分隔
    for (int i = 0; i < n; ++i) {
        if (i) cout << ' ';
        cout << ans[i];
    }
    cout << '\n';
    return 0;
}
```

