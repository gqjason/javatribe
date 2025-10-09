>每日抽象文案：我发誓我再也不玩抽象了，十月该好好洗涤自己， 挑战今天不发抽象文案



# 每日两题
---


# 一、基础题
### 题目：[P1035 [NOIP 2002 普及组] 级数求和](https://www.luogu.com.cn/problem/P1035)
### 思路：
不断循环相加即可
### 代码(c语言)：
时间复杂度 **O(n)**，其中 n 为使级数和首次超过 k 时的项数。
```c
#include <stdio.h>

int main() {
    double k;
    scanf("%lf", &k);
    double sum = 0.0;
    int i = 1;
    while (1) {
        sum += 1.0 / i;
        if (sum > k) {
            printf("%d\n", i);
            break;
        }
        i++;
    }
    return 0;
}
```

# 二、提高题
### 题目：[P1090 [NOIP 2004 提高组] 合并果子](https://www.luogu.com.cn/problem/P1090)
### 思路：
本题是典型的贪心【哈弗曼树】问题。每次合并都选择当前最小的两堆果子，这样可以保证每一步的代价最小，从而使总花费最小。实现时可以用小根堆（优先队列）维护所有果子的堆数（也可以用数组的二分插入），每次取出最小的两个合并，再将新堆放回堆中，直到只剩下一堆为止。

🤔当然，不会优先队列的也可以用暴力去解答，时间复杂度为 **O($n^2$)** 。由于n范围为$1 \leq n \leq 10000$，在一秒的时间限制内做点小优化也是可以过的。

**三倍经验**：[P6033 [NOIP 2004 提高组] 合并果子 加强版](https://www.luogu.com.cn/problem/P6033)
**四倍经验**：[P1775 石子合并（弱化版）](https://www.luogu.com.cn/problem/P1775)
**五倍经验**：[P1880 [NOI1995] 石子合并](https://www.luogu.com.cn/problem/P1880)

可能需要用到的知识：
1. 优先队列：
学习网站 [(csdn)【原创】优先队列 priority_queue 详解](https://blog.csdn.net/c20182030/article/details/70757660)，b站: [【从堆的定义到优先队列、堆排序】 10分钟看懂必考的数据结构——堆【工程部老周】](https://www.bilibili.com/video/BV1AF411G7cA/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
2. 二分
学习网站 [OI Wiki](https://oi-wiki.org/basic/binary/)，b站: [蛋从几层楼往下丢才会碎？一个视频讲明白【NotOnlySuccess】](https://www.bilibili.com/video/BV1DzXzYPEYT/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f) 

### 代码(c++)：
时间复杂度 **O($n \log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n;
    cin >> n;
    priority_queue<int, vector<int>, greater<int>> pq;//小根堆
    for (int i = 0; i < n; ++i) {
        int x;
        cin >> x;
        pq.push(x);
    }
    int total_cost = 0;
    while (pq.size() > 1) {
        int a = pq.top(); pq.pop();
        int b = pq.top(); pq.pop();
        total_cost += a + b;
        pq.push(a + b);
    }
    cout << total_cost << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```


