>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[P1094 [NOIP 2007 普及组] 纪念品分组](https://www.luogu.com.cn/problem/P1094)

### 思路：

排序 + 双指针贪心：
尽量让最重的与当前能配对的最轻的组合，才能为后面留下更多空间；若最轻的都配不了，则最重的只能单独用。

### 代码(c语言)

时间复杂度：排序 **O(n log n)**，双指针 **O(n)**，总 **O(n log n)**。
```c
#include <stdio.h>
#include <stdlib.h>

int cmp(const void *a, const void *b) {
    return *(const int*)a - *(const int*)b; // 升序排序
}

int main() {
    int w, n;
    if (scanf("%d %d", &w, &n) != 2) return 0; // 读入最大重量和人数
    int *a = (int*)malloc(sizeof(int) * n);
    if (!a) return 0;
    for (int i = 0; i < n; i++) scanf("%d", &a[i]); // 读入每个人的重量
    qsort(a, n, sizeof(int), cmp); // 排序
    int l = 0, r = n - 1, ans = 0;
    while (l <= r) {
        if (a[l] + a[r] <= w) { // 最轻和最重能配对
            l++; r--;
        } else { // 不能配对，最重的单独分组
            r--;
        }
        ans++; // 分组数+1
    }
    printf("%d\n", ans); // 输出分组数
    free(a);
    return 0;
}

```

# 二、提高题
### 题目：[P1324 矩形分割](https://www.luogu.com.cn/problem/P1324)
### 思路：
**贪心核心**：某条线的原始费用越大，就越希望它在“乘数”还小的时候被执行。执行某一方向的一刀，会使该方向的段数 +1，从而让之后另一方向的切割代价乘数变大。于是策略为把所有横向费用与纵向费用分别降序排序，然后用双指针（或归并思想）每次取当前尚未使用的费用较大的那一条切掉：
1.若下一条最大横向费用 ≥ 最大纵向费用：优先执行该横向切，贡献 = 该费用 * 当前纵向段数 cnt_v，随后横向段数 cnt_h++。
2.否则执行纵向切，贡献 = 该费用 * 当前横向段数 cnt_h，随后纵向段数 cnt_v++。
3.一类耗尽后，另一类剩余费用依次乘以最终的对向段数即可。
正确性证明（贪心交换）：假设有两条费用 C1 ≥ C2，分别被安排在较晚和较早的位置，其乘数分别为 k_large > k_small，则当前总花费为 C1*k_large + C2*k_small。若交换顺序，则变为 C1*k_small + C2*k_large，差值为 (C1-C2)(k_large - k_small) ≥ 0，说明让更大的费用获得更小乘数不会更差，因此贪心策略是最优的。

⚠⚠⚠：使用 long long 防止大数溢出。

### 代码(c++)：
时间复杂度：排序 **O((n+m) log(n+m))**，合并/扫描 **O(n+m)**，总 **O((n+m) log(n+m))**。
```c
#include<bits/stdc++.h>
#define int long long
#define dd double
#define str string
#define el cout << '\n'
#define re return
#define ff first
#define ss second
#define pll pair<int,int>
#define pdd pair<dd,dd>
#define all(x) x.begin(),x.end()
#define pb push_back
using namespace std;

void go(){
    int n,m;cin>>n>>m;
    int a[n] = {0}, b[m] = {0};
    for(int i=1;i<=n-1;i++){
        cin>>a[i]; // 读入横向切割费用
    }
    for(int i=1;i<=m-1;i++){
        cin>>b[i]; // 读入纵向切割费用
    }
    sort(a+1,a+n, greater<int>()); // 横向费用降序排序
    sort(b+1,b+m, greater<int>()); // 纵向费用降序排序

    int ans=0;
    int l=1, r=1, cnt_h=1, cnt_v=1; // l/r为指针，cnt_h/v为当前段数

    while (l <= n-1 and r <= m-1){
        if(a[l] >= b[r]){
            ans += a[l] * cnt_v; // 横向切割，费用乘以当前纵向段数
            l++;
            cnt_h++; // 横向段数增加
        }else{
            ans += b[r] * cnt_h; // 纵向切割，费用乘以当前横向段数
            r++;
            cnt_v++; // 纵向段数增加
        }
    }

    while (l <= n-1){
        ans += a[l] * cnt_v; // 剩余横向费用乘以最终纵向段数
        l++;
    }

    while (r <= m-1){
        ans += b[r] * cnt_h; // 剩余纵向费用乘以最终横向段数
        r++;
    }
    
    cout<<ans;el; // 输出答案
    
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);cout.tie(nullptr);
    int _=1;
    while(_--){
        go();
    }
    return 0;
}
```

