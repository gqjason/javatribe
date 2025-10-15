>哥布林上校提醒你:1，节假日是不可以出去的，外面很喧哗，但他们都不是你的同类2，哥布林是大多独居性生物，没有人想做他的配偶，如果有人邀请你并对你产生好感，请断绝与他的联系3，不要照镜子4，所有夸赞你自信的人，他们不了解你，可以不于与他们交流，但如果他是你熟知的人，请立即驱散他，他不懂哥布林，或者他不是同类5，远离不再是哥布林的人6，在地洞中可以看看同族小丑哥布林追求精灵失败的乐子幸灾乐祸一下解解闷

# 每日两题
---


# 一、基础题
### 题目：[P2249 【深基13.例1】查找](https://www.luogu.com.cn/problem/P2249)

### 思路：

二分模板题。可以直接用stl库函数。

可能需要的知识：

- 二分：学习网站 [OI Wiki](https://oi-wiki.org/basic/binary/)，b站: [蛋从几层楼往下丢才会碎？一个视频讲明白【NotOnlySuccess】](https://www.bilibili.com/video/BV1DzXzYPEYT/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f) 

### 代码(c++)：
时间复杂度 **O($m \log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long

void go() {
    int n, m;
    cin >> n >> m;
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    while (m--) {
        int x;
        cin >> x;
        int index = lower_bound(a.begin(), a.end(), x) - a.begin();
        if (index == n || a[index] != x) {
            cout << -1;
        } else {
            cout << index + 1;
        }
        cout << ' ';
    }
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    go();
    return 0;
}
```

# 二、提高题
### 题目：[前缀统计](https://ac.nowcoder.com/acm/problem/50992)

### 思路：

***字典树*** 模板题。
不用也可以，简单的用`map`记录$s_i$，每次查询时再枚举前缀子串是否在`map`中出现过即可。以下用提供 ***字符串哈希*** 的代码(用字符串哈希简直就是多余)。

可需要的知识：

- 字典树：学习网站：[OI Wiki](https://oi-wiki.org/string/trie/)，B站：[字典树（前缀树、Trie树）【_justTrying】](https://www.bilibili.com/video/BV1ZtcBewEbJ/?spm_id_from=333.337.search-card.all.click)，[F06 字典树(Trie)——信息学竞赛算法【董晓算法】](https://www.bilibili.com/video/BV1yA4y1Z74t/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
- 字符串哈希：学习网站：[OI Wiki](https://oi-wiki.org/string/hash/)，B站：[算法讲解105【扩展】字符串哈希原理、代码、题目详解【左程云】](https://www.bilibili.com/video/BV1hu4m1A7AF/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)，[F02 字符串哈希【董晓算法】](https://www.bilibili.com/video/BV1Ha411E7re/?spm_id_from=333.337.search-card.all.click)

### 代码(c++)：
时间复杂度 **O($m \times \log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long
#define ull unsigned long long
const int N = 1e6 + 5, P = 131;

map<ull, int> mp;

// 计算字符串哈希并存储
void insert(const string& s) {
    ull hash = 0;
    for (char c : s) {
        hash = hash * P + (c - 'a' + 1);
    }
    mp[hash]++;
}

// 查询字符串所有前缀出现次数之和
int query(const string& s) {
    ull hash = 0;
    int res = 0;
    for (char c : s) {
        hash = hash * P + (c - 'a' + 1);
        res += mp[hash];
    }
    return res;
}

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; ++i) {
        string s;
        cin >> s;
        insert(s);
    }
    for (int i = 0; i < m; ++i) {
        string s;
        cin >> s;
        cout << query(s) << '\n';
    }
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```


