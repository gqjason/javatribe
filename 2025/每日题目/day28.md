>是的，我仅仅用了一年就从大二学生变成了大三学生，这其中的艰苦你们是不会懂的，你们不知道有多少个难眠的夜晚，我为了这个目标暗暗努力。

# 每日两题
---


# 一、基础题
### 题目：[小苯的数字折叠1.0](https://ac.nowcoder.com/acm/problem/300162)

### 思路：
与 [P1015 [NOIP 1999 普及组] 回文数](https://www.luogu.com.cn/problem/P1015)【每日两题day23】 题目相似，不过条件不同。
直接模拟判断即可。
以下提供python代码(py还是太遥遥领先了)。
### 代码(c++)：
时间复杂度 **O($len(n) \cdot k$)**，其中 $len(n)$ 为`n`的位数

```py
def cal(n):
    rn = int(str(n)[::-1])
    n += rn
    return n
    
def check(n):
    cn = n
    return str(n) == str(cn)[::-1]

def go():
    n,k = map(int,input().split())
    res = n
    if check(res):
        print(f"{res} {0}")
        return
    
    for i in range(1,k+1):
        res = cal(res)
        if check(res):
            print(f"{res} {i}")
            return
        
    print(f"{res} {-1}")
    
if __name__ == '__main__':
    t = int(input())
    for i in range(0,t):
        go()
```

# 二、提高题
### 题目：[P5461 赦免战俘](https://www.luogu.com.cn/problem/P5461)

### 思路：

本题是一个递归构造二维矩阵的题目。
正难则反，发现填1的代码比较繁琐，思考能否反过来填0。
初始化一个 $2^n \times 2^n$ 的矩阵。初始时所有位置为 $1$，然后每次将当前矩阵分成四个相等的小矩阵，把左上角的小矩阵全部置为 $0$，递归处理剩下的三个小矩阵。递归终止条件是矩阵大小为 $1 \times 1$。

可能需要的知识：
- dfs：学习网站：[OI Wiki](https://oi-wiki.org/search/dfs/)，B站：[图的算法-DFS深度优先遍历搜索算法 数据结构与算法【图码】](https://www.bilibili.com/video/BV17Y4UefEzs/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)
- 递归：学习网站：[OI Wiki](https://oi-wiki.org/basic/divide-and-conquer/)，B站：[【递归1】递归中的逆向思维【五点七边】](https://www.bilibili.com/video/BV1214y157HG?spm_id_from=333.788.recommend_more_video.-1&vd_source=933c136d6897dbf20ff125fb1209208f)，[有个说法：“「递归」是检验编程天赋的试金石”；而本视频打破天赋壁垒，助你快速掌握递归。【NotOnlySuccess】](https://www.bilibili.com/video/BV1LiS1YSEgF/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f)

### 代码(c++)：
时间复杂度 **O(3^n)**，因为每次递归分成四块，递归深度为 $n$。

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, up;
vector<vector<int>> board;

void dfs(int l, int r, int u, int d) {
    if (l == r && u == d) {
        return;
    }

    int ch = (l + r) / 2, rh = (u + d) / 2;

    for (int i = l; i <= ch; i++) {
        for (int j = u; j <= rh; j++) {
            board[i][j] = 0;
        }
    }

    
    dfs(ch + 1, r, u, rh);//右上
    dfs(l, ch, rh + 1, d);//左下
    dfs(ch + 1, r, rh + 1, d);//右下
}

void solve() {
    cin >> n;
    up = 1 << n;
    board.assign(up + 1, vector<int>(up + 1, 1));

    dfs(1, up, 1, up);

    for (int i = 1; i <= up; i++) {
        for (int j = 1; j <= up; j++) {
            cout << board[i][j] << ' ';
        }
        cout << '\n';
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

