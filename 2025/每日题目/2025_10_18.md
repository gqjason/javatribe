>坚持，是一种品格

# 每日两题
---


# 一、基础题
### 题目：[P4715 【深基16.例1】淘汰赛](https://www.luogu.com.cn/problem/P4715)
### 思路：
由于n不大，暴力即可。每次选择相邻两个数进行对比，对较大的那个放入新的数组中，重复n次，就能得出亚军。
### 代码(c++)：
方法1(暴力)：
时间复杂度 **O($2^n$)**  
```cpp
#include <iostream>
#include <vector>
using namespace std;

void go() {
    int n;
    cin >> n;
    int total = 1 << n;
    vector<pair<int, int>> a(total + 1);
    for (int i = 1; i <= total; i++) {
        cin >> a[i].first;//输入
        a[i].second = i;//记录下标
    }
    int rounds = n;
    while (rounds-- > 1) {
        vector<pair<int, int>> b(total / 2 + 1);
        int idx = 1;
        for (int i = 1; i <= total; i += 2) {
            //每次选择相邻较大的数
            if (a[i].first > a[i + 1].first) {
                b[idx++] = a[i];
            } else {
                b[idx++] = a[i + 1];
            }
        }
        total /= 2;
        a = b;
    }

    if (a[1].first > a[2].first) {
        cout << a[2].second << endl;
    } else {
        cout << a[1].second << endl;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    cin >> t;
    while (t--) {
        go();
    }
    return 0;
}
```
方法2(递归)：
时间复杂度 **O($2^n$)** 
```cpp
#include <iostream>
#include <vector>
using namespace std;

const int N = 1000;
int n;
vector<int> a(N, 0);

int search(int l, int r) {
    if (l == r) {
        return l;//当只有一个数时，只返回该数的下标
    }
    int mid = (l + r) / 2;
    int left = search(l, mid);
    int right = search(mid + 1, r);
    return (a[left] > a[right] ? left : right);//每轮返回较大数的下标
}

void go() {
    cin >> n;
    int total = 1 << n;
    for (int i = 1; i <= total; i++) {
        cin >> a[i];
    }
    int mid = total / 2;
    int left_champion = search(1, mid);//得到左边冠军的下标
    int right_champion = search(mid + 1, total);//得到右边冠军的下标
    if (a[left_champion] > a[right_champion]) {
        cout << right_champion << endl;
    } else {
        cout << left_champion << endl;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int t = 1;
    // cin >> t;
    while (t--) {
        go();
    }
    return 0;
}
```
方法3：直接找左右两半的最大值，对比输出即可
这里就不亮出代码了🙂


# 二、提高题
### 题目：[P1168 中位数](https://www.luogu.com.cn/problem/P1168)
### 思路：
题目要求在每输入一个数后，输出当前所有数中前奇数项的中位数。可以用两个堆（优先队列）来动态维护中位数：  
- 用一个大根堆（max-heap）存较小的一半元素，一个小根堆（min-heap）存较大的一半元素。  
- 每次插入新元素后，保持大根堆的元素个数不少于小根堆，且两者个数最多相差1。  
- 这样每当输入了奇数个数时，大根堆的堆顶就是当前的中位数，直接输出即可。  

可能需要用到的知识：
1. 优先队列：
学习网站 [(csdn)【原创】优先队列 priority_queue 详解](https://blog.csdn.net/c20182030/article/details/70757660)，b站视频 [【从堆的定义到优先队列、堆排序】 10分钟看懂必考的数据结构——堆【工程部老周】](https://www.bilibili.com/video/BV1AF411G7cA/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
2. 二分
学习网站 [OI Wiki](https://oi-wiki.org/basic/binary/)，b站视频 [蛋从几层楼往下丢才会碎？一个视频讲明白【NotOnlySuccess】](https://www.bilibili.com/video/BV1DzXzYPEYT/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f) 

### 代码(c++)：
时间复杂度：**O($\log n$)**

```cpp
#include <bits/stdc++.h>
using namespace std;

void go() {
    int n;
    cin >> n;
    vector<int> a(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    priority_queue<int> mx; // 大根堆，存较小的一半
    priority_queue<int, vector<int>, greater<int>> mi; // 小根堆，存较大的一半

    for (int i = 1; i <= n; i++) {
        if (mx.empty() || a[i] <= mx.top()) {
            mx.push(a[i]);
        } else {
            mi.push(a[i]);
        }
        // 保证mx比mi多0或1个
        if (mx.size() > mi.size() + 1) {
            mi.push(mx.top());
            mx.pop();
        } else if (mx.size() < mi.size()) {
            mx.push(mi.top());
            mi.pop();
        }
        if (i % 2 == 1) {
            cout << mx.top() << '\n';
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    go();
    return 0;
}
```
当然，也可以简单的二分插入
时间复杂度：**O($n^2$)**
(至于为什么能过,可能是因为数据太水了🤣🤣🤣)
```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
vector<int>a;
int main()
{
    cin>>n;
    for(int i=1,x;i<=n;i++)
    {
        scanf("%d",&x);
        a.insert(upper_bound(a.begin(),a.end(),x),x);//二分插入保证单调性
        if(i%2==1)
        {
            printf("%d\n",a[(i-1)/2]);//是奇数个就输出
        }
    }
    return 0;
}
```
