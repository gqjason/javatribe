>玩抽象真的是走投无路了，青春期长的磕碜，又毫无欣赏的特长，不得不辛苦的给自己培养出似乎很有趣的性格，拿自己开涮，不玩点烂梗真没人叼我了

# 每日两题
---


# 一、基础题
### 题目：[P10910 [蓝桥杯 2024 国 B] 最小字符串](https://www.luogu.com.cn/problem/P10910)

### 思路：
- 将待插入串按字典序升序排序。
- 用双指针在原串与排序后的待插入串上做贪心合并：每次取更小的字符追加；谁小取谁。
- 若两边当前字符相等，为避免局部最优导致整体非最优，做一次前瞻比较：同步向后比较两个后缀直到某一方结束或出现不同字符；若待插入串先用尽，或原串的下一个更小，则取原串当前字符，否则取待插入串当前字符。
- 最后把未用完的一侧全部追加即可。

### 代码(c++)：
时间复杂度 **O(n+m)**

```cpp
#include <bits/stdc++.h>
#define int long long
#define str string
#define all(x) x.begin(),x.end()
using namespace std;

void go() {
    int n, m;
    cin >> n >> m;
    str s1, s2;
    cin >> s1 >> s2;

    sort(all(s2)); // 把待插入字符排序（升序）
    int i = 0, j = 0;
    str ans;

    while (i < n && j < m) {
        if (s1[i] < s2[j]) {
            ans += s1[i++];
        } else if (s1[i] > s2[j]) {
            ans += s2[j++];
        } else {
            // s1[i] == s2[j]，需要比较后缀
            int ii = i, jj = j;
            while (ii < n && jj < m && s1[ii] == s2[jj]) {
                ii++; jj++;
            }
            bool chooseS1 = false;
            if (jj == m) chooseS1 = true; // s2 先用完 -> s1 后缀更小
            else if (ii < n && s1[ii] < s2[jj]) chooseS1 = true;

            if (chooseS1) ans += s1[i++];
            else ans += s2[j++];
        }
    }

    // 把剩下的字符拼上
    while (i < n) ans += s1[i++];
    while (j < m) ans += s2[j++];

    cout << ans << '\n';
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    go();
    return 0;
}

```

# 二、提高题
### 题目：[P1246 编码](https://www.luogu.com.cn/problem/P1246)

### 思路：
由于查询的字符串长度最大为6，可以考虑暴力(暴力才是最屌的🤣)
- 用 DFS 递归生成所有合法字符串，并用 map 记录每个字符串对应的编号。
- 查询时直接输出输入字符串的编号即可。
- 时间复杂度为所有长度为 1~6 的组合数之和，即 $C^{26}_1 + C^{26}_2 + ... + C^{26}_6$，约 230,000，完全可行。

### 代码(c++)：
时间复杂度 **O($\sum^{i}_{1 \leq i \leq 6} C^{26}_{i}$)**

```cpp
#include <iostream>
#include <map>
using namespace std;

int cnt = 1;
map<string, int> mp;
string ask;

void dfs(string s, int len, int mx) {
    if (len == mx){
        mp[s] = cnt++;
        return;
    } 

    char start = s.empty() ? 'a' : s.back() + 1;
    for (char i = start; i <= 'z'; i++) {
        dfs(s + i, len + 1, mx);
    }
}

int main() {
    for(int len=1;len<=6;len++){
        dfs("", 0, len);
    }

    cin >> ask;
    cout << mp[ask] << endl;
    return 0;
}

```

