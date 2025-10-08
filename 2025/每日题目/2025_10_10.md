>可不可以不要这么抽象，你抽象，象怎么想？象也是生命啊

# 每日一题

---


# 一、基础题
### 题目：[P1055 [NOIP 2008 普及组] ISBN 号码](https://www.luogu.com.cn/problem/P1055)

### 思路：
本题主要考察对字符串的处理。先将字符串分成以'-'号分成四份，再按照题目要求对第三份进行计算，将结果与第四份比较，如果相同则输出"right"，否则输出修改后正确的ISBN号码。

### 代码(c语言)：
时间复杂度 **O(n)**

```cpp
#include <stdio.h>
#include <string.h>

void go() {
    char s[20];
    scanf("%s", s);
    
    int total = 0;
    int j = 1;
    int len = strlen(s);
    
    for (int i = 0; i < len - 1; i++) {
        if (s[i] == '-') {
            continue;
        }
        
        int se;
        if (s[i] == 'X') {
            se = 10;
        } else {
            se = s[i] - '0';
        }
        
        total += se * j;
        j++;
    }
    
    int fo;
    if (s[len - 1] == 'X') {
        fo = 10;
    } else {
        fo = s[len - 1] - '0';
    }
    
    char ri;
    if (total % 11 == 10) {
        ri = 'X';
    } else {
        ri = (total % 11) + '0';
    }
    
    if (total % 11 == fo) {
        printf("Right\n");
    } else {
        s[len - 1] = ri;
        printf("%s\n", s);
    }
}

int main() {
    go();
    return 0;
}
```

# 二、提高题
### 题目：[P3397 地毯](https://www.luogu.com.cn/problem/P3397)

### 思路：

本题主要考察差分和前缀和的知识（如果不会，也可以靠暴力去求解，毕竟样例挺水的(bushi)）。
先初始化一个二位数组,对于每次输入的x1,y1,x2,y2，仅修改 4 个差分位置：cnt[x1][y1]++、cnt[x2+1][y2+1]++、cnt[x1][y2+1]--、cnt[x2+1][y1]--，时间复杂度为O(1)。
最后，对差分数组求二维前缀和（公式：cnt[i][j] += cnt[i][j-1] + cnt[i-1][j] - cnt[i-1][j-1]），就能得出最终答案。

可能需要的知识：
- 前缀和差分:
学习网站： [OI Wiki](https://oi-wiki.org/basic/prefix-sum/) ，b站：[前缀和及其变种，十道题检验你是不是真的学会了【NotOnlySuccess】](https://www.bilibili.com/video/BV1xKfyY9EkN/?spm_id_from=333.1387.upload.video_card.click&vd_source=933c136d6897dbf20ff125fb1209208f) 。

### 代码(c++)：
时间复杂度 **O($n^2$)**

```cpp
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
using namespace std;

void go(){
    int n,m;cin>>n>>m;
    vector<vector<int>>cnt(n+2,vector<int>(n+2,0));

    for(int i=0;i<m;i++){
        int xx1,yy1,xx2,yy2;
        cin>>xx1>>yy1>>xx2>>yy2;

        cnt[xx1][yy1]++;
        cnt[xx2+1][yy2+1]++;
        cnt[xx1][yy2+1]--;
        cnt[xx2+1][yy1]--;
    }

    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cnt[i][j] += cnt[i][j-1]+cnt[i-1][j]-cnt[i-1][j-1];
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cout<<cnt[i][j]<<' ';
        }el;
    }

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

