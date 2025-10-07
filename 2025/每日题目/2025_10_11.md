>坚持，是一种品格

# 每日一题
---


# 一、基础题
### 题目：[P1093 [NOIP 2007 普及组] 奖学金](https://www.luogu.com.cn/problem/P1093)
### 思路：
遍历每个学生，计算三科总分。将所有学生按总分从高到低排序，总分相同则按语文成绩高者优先，若仍相同则按学号升序。输出前五名的学号和总分。
### 代码(c语言)：
时间复杂度 **O(n)**
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int id;
    int score1;
    int score2;
    int score3;
    int total;
} Grade;

int cmp(const void *a, const void *b) {
    Grade *ga = (Grade *)a;
    Grade *gb = (Grade *)b;
    if (ga->total != gb->total)
        return gb->total - ga->total;
    else if (ga->score1 != gb->score1)
        return gb->score1 - ga->score1;
    else
        return ga->id - gb->id;
}

int main() {
    int n;
    scanf("%d", &n);
    Grade *grades = (Grade *)malloc(n * sizeof(Grade));
    for (int i = 0; i < n; i++) {
        grades[i].id = i + 1;
        scanf("%d%d%d", &grades[i].score1, &grades[i].score2, &grades[i].score3);
        grades[i].total = grades[i].score1 + grades[i].score2 + grades[i].score3;
    }
    qsort(grades, n, sizeof(Grade), cmp);
    int m = n < 5 ? n : 5;
    for (int i = 0; i < m; i++) {
        printf("%d %d\n", grades[i].id, grades[i].total);
    }
    free(grades);
    return 0;
}

```

# 二、提高题
### 题目：[P1007 独木桥](https://www.luogu.com.cn/problem/P1007)
### 思路：
遍历每只蚂蚁，计算其到桥两端的距离，最短时间为所有蚂蚁到最近端的最大值，最长时间为所有蚂蚁到最远端的最大值。即最短时间为 $\max(\min(a_i, L-a_i+1))$，最长时间为 $\max(\max(a_i, L-a_i+1))$，其中 $a_i$ 为第 $i$ 只蚂蚁的位置，$L$ 为桥长。
### 代码(c++)：
时间复杂度 **O(n)**
```c
#include<bits/stdc++.h>
#define int long long
#define dd double
#define str string
#define el cout << endl
#define re return
#define ff first
#define ss second
#define pll pair<int,int>
#define pdd pair<dd,dd>
#define all(x) x.begin(),x.end()
#define pb(x) push_back(x)
#define sz(x) (int) x.size()
using namespace std;

void go(){
    int l,n;cin>>l>>n;
    vector<int> a(n);
    for (size_t i = 0; i < n; i++)
    {
        cin>>a[i];
    }
    if(n == 0){cout<<0<<' '<<0;re;}
    sort(all(a));
    int minans = INT32_MIN,maxans = INT32_MIN;
    maxans = max(maxans,max(l-a[0]+1,a[n-1]));
    for(int i=0;i<n;i++){
        minans = max(minans,min(l-a[i]+1,a[i]));
    }
    cout<<minans<<' '<<maxans;el;
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);cout.tie(nullptr);
    int t=1;
    //cin >>t;
    while(t--){
        go();
    }
    return 0;
}
```

