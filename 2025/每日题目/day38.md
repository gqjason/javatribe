>我真的不想发这些屌丝段子了 可是我除了这些还能发什么啊

# 每日两题
---


# 一、基础题
### 题目：[P1167 刷题](https://www.luogu.com.cn/problem/P1167)

### 思路：
- 先把起止时间字符串解析成绝对分钟数：判断闰年，预处理每月天数前缀和，再做 yyyy-mm-dd hh:mm → 分钟的转换。
- 计算可用总时长 dis = end − start。
- 将每题用时升序排序，按贪心从小到大依次取题，累加直至超过 dis，取到的题数即答案。

### 代码(c++)：
时间复杂度 **O($n \log n$)**，不算预处理的时间复杂度，仅为排序的时间复杂度。
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

// year[i] 表示从公元 0 年开始到 i 年结束累计的“天数”
int year[10010];

// 闰年与平年每月天数表（1-based，下标 1..12）
int runnian[20] = {0,31,29,31,30,31,30,31,31,30,31,30,31};
int pingnian[20] = {0,31,28,31,30,31,30,31,31,30,31,30,31};

// 月份前缀和：sum_run[i] 为闰年 1..i 月的累计天数；sum_ping 同理
int sum_run[20], sum_ping[20];

// 判闰年：400 的倍数是闰年；100 的倍数非闰；否则 4 的倍数是闰年
bool isLeap(int y){
    if(y%400==0) return true;
    if(y%100==0) return false;
    return y%4==0;
}

// 预处理：
// 1) 预计算两种年份的“月份前缀天数”
// 2) year[i] 为从 0 年到 i 年结束的总天数（便于把日期转为绝对“天”）
void pre(){
    for(int i=1;i<=12;i++){
        sum_run[i] = sum_run[i-1] + runnian[i];
        sum_ping[i] = sum_ping[i-1] + pingnian[i];
    }
    for(int i=1;i<10010;i++){
        year[i] = year[i-1] + (isLeap(i)?sum_run[12]:sum_ping[12]); // i 年的总天数
    }
}

// 将 "YYYY-MM-DD HH:MM" 转为“自公元 0-01-01 00:00 起的绝对分钟数”
int cal(string s){
    // 位置严格依赖固定格式：0..3 年，5..6 月，8..9 日，11..12 时，14..15 分
    int yy = stoi(s.substr(0,4));
    int mm = stoi(s.substr(5,2));
    int dd = stoi(s.substr(8,2));
    int hh = stoi(s.substr(11,2));
    int mi = stoi(s.substr(14,2));

    // 绝对“天” = 之前整年的天数 + 当年之前整月的天数 + 当月天
    int date = year[yy-1] + (isLeap(yy)?sum_run[mm-1]:sum_ping[mm-1]) + dd;
    // 转分钟
    int res = date*24*60 + hh*60 + mi;
    return res;
}

void go(){
    pre();
    int n; cin >> n;                      // 题目数量
    vector<int>a(n);
    for(int i=0;i<n;i++) cin >> a[i];    // 每题用时（分钟）
    sort(a.begin(), a.end());            // 升序，便于贪心

    string sta, end;
    cin >> sta >> end;


    int st = cal(sta), et = cal(end);    // 转绝对分钟
    int dis = et - st;                   // 可用总时长（分钟）
    
    int cnt=0;                           // 能完成的题数
    for(int i=0; i<n && dis>=a[i]; i++){ // 从最短的题开始选
        cnt++;
        dis -= a[i];
    }
    cout << cnt << "\n";                 // 输出最大可完成题数
}

signed main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    go();                                // 入口
}
```

# 二、提高题
### 题目：[小苯的数字切割](https://ac.nowcoder.com/acm/problem/291357)

### 思路：
将数字串在任意位置切成左右两段，分别按十进制转为整数，取所有切法中左数与右数之和的最大值。实现上从右向左移动切割线：初始化 l 为除末位外的前缀数，r 为末位；每次把 l 的末位 t 移到 r 的最高位（r += t * 10^k，k 为已移动的位数），同时 l /= 10，并用 l + r 更新答案。这样可在一次线性扫描内枚举所有切法。

### 代码(c++)：
时间复杂度 **O(n)** 
```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

void solve() {
    string s;
    cin >> s;
    int n = (int)s.size();

    ll l = 0, r = 0;
    for (int i = 0; i < n; ++i) {
        if (i < n - 1) l = l * 10 + (s[i] - '0');
        else r = s[i] - '0';
    }

    ll ans = l + r;           // 初始切法：前缀 | 末位
    ll pow10 = 10;            // 已移动位数对应的 10 的幂

    for (int i = 0; i < n - 2; ++i) {
        ll t = l % 10;        // 将 l 的末位移动到 r 的最高位
        l /= 10;
        r += t * pow10;
        pow10 *= 10;
        ans = max(ans, l + r);
    }

    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int T = 1;
    if (!(cin >> T)) return 0;
    while (T--) solve();
    return 0;
}
```

