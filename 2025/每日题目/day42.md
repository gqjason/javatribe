>😴😴😴

# 每日两题
---


# 一、基础题
### 题目：[76修地铁](https://ac.nowcoder.com/acm/problem/297381)

### 思路：
计算题。依照题目要求计算即可。

### 代码(c++)：
时间复杂度 **O(1)**

```cpp
#include<bits/stdc++.h>
using namespace std;

void go(){
    int n;cin>>n;
    int a,b,c,d;
    a =(n/5) * 2;//普通火把
    b = (n-5)/10 + (n>=5);//红石火把
    c = n/20;
    d = n*2 - c*2;//动力铁轨
    c *= 3;//普通铁轨
    cout<<a<<' '<<b<<' '<<c<<' '<<d<<endl;
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

# 二、提高题
### 题目：[76选数](https://ac.nowcoder.com/acm/problem/297879)

### 思路：
贪心构造题。异或运算的性质是：当 $a = b$ 时 $a \oplus b = 0$，当 $a \neq b$ 时 $a \oplus b = 1$。将 $1$ 到 $n$ 的所有数都转为二进制后，为了让异或和最大，我们希望选择的数字组合中，每一位上 $1$ 的个数为奇数。
具体做法是：从 $1$ 开始，每次选择当前未被选择的最大 $2$ 的幂（即 $1(1_2), 2(10_2), 4(100_2), 8(1000_2), \ldots$），直到超过 $n$ 为止。因为每个 $2$ 的幂在二进制中只有一位是 $1$，这样选出来的数异或和能覆盖所有高位，保证结果最大。
所以，答案就是 $1 + 2 + 4 + \cdots + k$，其中 $k$ 是不超过 $n$ 的最大 $2$ 的幂。


可能需要的知识：
- 位运算：[OI Wiki](https://oi-wiki.org/math/bit/), B站：[算法讲解003【入门】二进制和位运算【左程云】](https://www.bilibili.com/video/BV1494y1C72o/?spm_id_from=333.337.search-card.all.click)

### 代码(c++)：
时间复杂度 **O($\log n$)**

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

void go(){
    int n;cin>>n;

    int tmp = 1;
    int ans = 0;
    while (tmp<=n)
    {
        ans += tmp;
        tmp *=2;
    }

    cout<<ans;el;
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

