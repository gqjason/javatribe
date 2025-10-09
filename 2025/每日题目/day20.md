>Patience is key in life

# 每日两题
---


# 一、基础题
### 题目：[小苯的数组重排1.0](https://ac.nowcoder.com/acm/problem/302581)
### 思路：
由题目公式 $S=(a_1+a_2)+(a_2+a_3)+\dots+(a_{n-1}+a_n)$ 可得，除了左右两边的数的贡献为1次，中间的数贡献为2次，所以我们可以让最小的两个数放两边，其他的放中间。
因此我们不必重构该数组，只需对数组从小到大排序，然后计算每个元素的贡献。对于第3个及之后的元素，每次多加一次该元素。

### 代码(c语言)：
时间复杂度 **O(nlogn)**
```c
#include <stdio.h>
#include <stdlib.h>

int cmp(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}

void go() {
    int n;
    scanf("%d", &n);
    int a[100005];
    for(int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    qsort(a, n, sizeof(int), cmp);
    long long sum = 0;
    for(int i = 0; i < n; i++) {
        sum += a[i];
        if(i > 1) {
            sum += a[i];
        }
    }
    printf("%lld\n", sum);
}

int main() {
    int t = 1;
    scanf("%d", &t);
    while(t--) {
        go();
    }
    return 0;
}
```

# 二、提高题
### 题目：[P5019 [NOIP 2018 提高组] 铺设道路](https://www.luogu.com.cn/problem/P5019)
### 思路：

题目要求将每个位置的高度都变为0，每次操作可以选择一个区间并将区间内所有数减1。最优策略是每次只处理高度变化的部分。我们可以统计每个位置比前一个位置高出的部分之和，最后加上第一个位置的高度。
1.**贪心策略**  
    观察每个位置与前一个位置的高度差：  
    (1) 如果当前位置比前一个高，说明需要额外操作来把多出来的部分减掉。  
    (2) 如果当前位置比前一个低或相等，则不需要额外操作，因为之前的操作已经覆盖了。
2.**举例说明**  
    假设数组为 `[2, 5, 3, 6]`：  
    (1) 第一个位置：操作2次  
    (2) 第二个位置比第一个高3（5-2），操作3次  
    (3) 第三个位置比第二个低，不操作  
    (4) 第四个位置比第三个高3（6-3），操作3次  
    总操作次数：2 + 3 + 3 = 8

### 代码(c++)：

时间复杂度 **O(n)**

```cpp

#include <iostream>
#include <vector>
using namespace std;

void go() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++){ 
        cin >> a[i];
    }
    long long ans = 0;
    for (int i = 1; i < n; i++) {
        // 如果当前位置比前一个高，需要额外操作
        if (a[i] > a[i - 1]) ans += a[i] - a[i - 1];
    }
    ans += a[0]; // 首位置直接加
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        go();
    }
    return 0;
}
```