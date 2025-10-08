>群里又哑巴了？每次看你们都聊得火热 但是只要我发了一句话 或者一个字 甚至一个表情 群里顿时就没了声音 怎么，我是灾星？瘟神？看到我就跑 是么？你们聊啊 我插不了嘴的 我一来你们就死群了，我观察了你们一早上从七点多开始聊到刚刚 我一出来就没人聊天了 有那么巧吗？😭😭😭


# 每日两题
---


# 一、基础题
### 题目：[P1996 约瑟夫问题](https://www.luogu.com.cn/problem/P1996)

### 思路：
数据范围太小，简单题简单做，直接模拟即可
这道题也是一道很好的练习链表的题目😎

可能需要的知识：

- 链表：
学习网站：[OI Wiki](https://oi-wiki.org/ds/linked-list/), b站：[链表 数据结构与算法，完整代码动画版，附在线数据结构交互式工具【图码】](https://www.bilibili.com/video/BV1ea4y1r75V/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
- 约瑟夫环扩展：
B站：[算法讲解146【扩展】康托展开、约瑟夫环、完美洗牌【左程云】](https://www.bilibili.com/video/BV1Dz2eYTE7T/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O($n^2$)**
```cpp
#include <iostream>
#include <vector>
using namespace std;

// 高效计算约瑟夫问题的出圈顺序
void josephus(int n, int m) {
    vector<int> people(n);  // 模拟圈中人的编号
    for (int i = 0; i < n; ++i) {
        people[i] = i + 1;  // 从 1 开始编号
    }

    vector<int> result;  // 存储出圈顺序
    int index = 0;       // 当前起始位置

    // 循环计算每次出圈的编号
    for (int remaining = n; remaining > 0; --remaining) {
        // 计算下一个出圈的位置
        index = (index + m - 1) % remaining;
        result.push_back(people[index]);
        people.erase(people.begin() + index);  // 从圈中移除
    }

    // 输出结果
    for (size_t i = 0; i < result.size(); ++i) {
        if (i > 0) cout << " ";
        cout << result[i];
    }
    cout << endl;
}

int main() {
    int n, m;
    cin >> n >> m;  // 输入人数和报数间隔
    josephus(n, m);
    return 0;
}
```

# 二、提高题
### 题目：[小红的双排列删除](https://ac.nowcoder.com/acm/problem/296528)
### 思路：
由题目可知，每次删除的区间需要首尾元素相等。
那么，我们会想，什么时候可以每次都删除首尾元素相等的区间直至该数组为空? 比如`a`:`1 2 4 1 5 3 2 4 5 3`是不可以的，因为无论我删哪个区间剩下都无法删除；而`b`:`1 3 4 4 1 2 3 2 5 6 7 6 7 5`是可以的，因为我们可以先删除首尾为`1`的区间，然后删除首尾为`2`的区间，最后删除首尾为`5`的区间。
**这时，我们会发现，只有当像`b`那样，可以分成多个相邻的独立的区间时，数组才能被清空。**
因此，解题流程为：从 $a_1$ 开始，找到这个数的另一个下标 $a_j$（满足 $a_j = a_i,\ j \neq i$），如果 $j < i$，则说明该数组无法被完全删除，再找$a_{j+1}$的另一个坐标，直至$j=2 \times n$。

例如：
\[
\begin{array}{lcccccccccccccc}
\text{数值}: & 1 & 3 & 4 & 4 & 1 & 2 & 3 & 2 & 5 & 6 & 7 & 6 & 7 & 5 \\
\text{下标}: & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10& 11& 12& 13& 14 \\
\end{array}
\]
$a_1$的相同数的另一个下标为 $j_0=5$，到下一个$a_{j_0+1}$的另一个下标为 $j_1=8$，到下一个 $a_{j_1+1}$ 的另一个下标为 $j_2=14$，当 $j=2 \times n$ 则表明该数组可以被清空。


### 代码(c++)：
时间复杂度 **O($n$)**

```cpp
#include <iostream>
#include <vector>
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<int> a(2 * n + 1, 0);
    vector<vector<int>> ind(n + 1);//储存每个数的下标
    ind[0] = {0, 0};
    for (int i = 1; i <= 2 * n; i++) {
        int x;
        cin >> x;
        a[i] = x;
        ind[x].push_back(i);
    }

    int il = a[1], ir = ind[il][1];//il表示a1的下标，ir表示a1的另一个下标
    while (ir < 2 * n) {
        int ni1 = a[ir + 1];//得到下一个数
        int ni2 = ind[ni1][0] + ind[ni1][1] - ir - 1;//得到下一个数的另一个下标
        if (ni2 < ir + 1) {//如果下一个数的另一个下标比下一个数的坐标小
            cout << "No" << endl;
            return;
        }
        il = ni1, ir = ni2;//更新
    }
    cout << "Yes" << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    int t = 1;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```

