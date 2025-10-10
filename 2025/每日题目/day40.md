>今天、明天、后天，都是不想上早八的一天

# 每日两题
---


# 一、基础题
### 题目：[小红与red](https://ac.nowcoder.com/acm/problem/296934)

### 思路：
遍历字符串,当发现 `s[i] == s[i-1]` 时,需要修改 `s[i]`
修改时要避免与前一个字符 `s[i-1]` 和后一个字符 `s[i+1]` 相同
使用 set 存储 {'r','e','d'},排除掉 `s[i-1]` 和 `s[i+1]`,剩下的字符就是可以使用的
在字符串末尾添加一个字符作为哨兵,方便处理边界情况,最后再删除

### 代码(c++)：
时间复杂度 **O(n)**

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    string s;
    if (!(cin >> n >> s)) return 0;

    if (n <= 1) {
        cout << s << '\n';
        return 0;
    }

    s.push_back(s.back()); // 哨兵，便于访问 s[i+1]
    for (int i = 1; i < n; ++i) {
        if (s[i] == s[i - 1]) {
            for (char c : string("red")) {
                if (c != s[i - 1] && c != s[i + 1]) {
                    s[i] = c;
                    break;
                }
            }
        }
    }
    s.pop_back(); // 去掉哨兵
    cout << s << '\n';
    return 0;
}
```

# 二、提高题
### 题目：[解码](https://ac.nowcoder.com/acm/problem/295851)

### 思路：
模拟题。用一个滑动窗口维护，当该窗口出现了对应的图形，则输出图形对应的按键，然后清空窗口。

### 代码(c++)：
时间复杂度 **O(n)**

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
#define pb push_back
#define sz(x) (int) x.size()
using namespace std;

const std::vector<std::vector<std::string> > FIGURES=
{
  {
    "#..",
    "##.",
    "#.."
  },{
    ".#.",
    ".##",
    ".#."
  },{
    ".#.",
    "##.",
    "#.."
  },{
    "..#",
    ".##",
    ".#."
  },{
    ".#.",
    "##.",
    ".#."
  },{
    "..#",
    ".##",
    "..#"
  },{
    "#..",
    "##.",
    ".#."
  },{
    ".#.",
    ".##",
    "..#"
  },{
    ".#.",
    "###"
  },{
    "##.",
    "##."
  },{
    ".##",
    ".##"
  },{
    "###",
    ".#."
  }//前8个图像3格高,后4个图像2格高
};
const std::vector<std::string> KEYS=
{
  "up","up","LT","LT","down","down","RT","RT",
  "left","A","A","right"//keys分别是对应的按键
};

vector<str>trans;
void tra(){
    for(int i=0;i<12;i++){
        str tmp = "";
        for(int j=0;j<FIGURES[i].size();j++){
            tmp += FIGURES[i][j];
        }
        //cout<<tmp;el;
        trans.push_back(tmp);
    }
}

int can(str t){
    for(int i=0;i<12;i++){
        if(t == trans[i]){
            re i;
        }
    }
    re -1;
}

void go(){
    int n;
    cin>>n;
    vector<string>v(n);
    for(int i=0;i<n;i++){
        cin>>v[i];
    }
    str t = "";
    for(int i=0;i<n;i++){
        t += v[i];
        if(can(t) != -1){
            cout<<KEYS[can(t)];el;
            t = "";
        }

    }
    el;

}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);cout.tie(nullptr);
    int t=1;
    //cin >>t;
    tra();
    while(t--){
        go();
    }
    return 0;
}
```

