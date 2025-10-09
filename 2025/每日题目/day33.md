>如果你花时间了解我的话，你就会发现，你浪费了一点时间。

# 每日两题
---


# 一、基础题
### 题目：[P1678 烦恼的高考志愿](https://www.luogu.com.cn/problem/P1678)

### 思路：

对于每个学生，要求选择一个学校，使得分数差的绝对值最小，并将所有学生的最小分数差累加。可以先将学校分数线排序，然后对每个学生用二分查找找到最接近的学校分数线，计算最小差值并累加。

可能需要的知识：
- 二分：学习网站：[OI Wiki](https://oi-wiki.org/basic/binary/)，b站: [蛋从几层楼往下丢才会碎？一个视频讲明白【NotOnlySuccess】](https://www.bilibili.com/video/BV1DzXzYPEYT/?spm_id_from=333.337.search-card.all.click&vd_source=933c136d6897dbf20ff125fb1209208f) 

### 代码(c++)：
时间复杂度 **O(n \log m)**，其中 $n$ 为学生人数，$m$ 为学校数量。

```cpp
#include <bits/stdc++.h>
using namespace std;

int m, n;
vector<int> school, student;

void solve() {
    cin >> m >> n;
    school.resize(m);
    student.resize(n);
    for (int i = 0; i < m; ++i) {
        cin >> school[i];
    }
    for (int i = 0; i < n; ++i) {
        cin >> student[i];
    }
    sort(school.begin(), school.end());
    long long ans = 0;
    for (int i = 0; i < n; ++i) {
        int idx = lower_bound(school.begin(), school.end(), student[i]) - school.begin();
        int diff = 0;
        if (idx >= m) {
            diff = abs(school[m - 1] - student[i]);
        } else if (idx == 0) {
            diff = abs(school[0] - student[i]);
        } else {
            diff = min(abs(school[idx] - student[i]), abs(school[idx - 1] - student[i]));
        }
        ans += diff;
    }
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    solve();
    return 0;
}
```

# 二、提高题
### 题目：[P1205 [USACO1.2] 方块转换 Transformations](https://www.luogu.com.cn/problem/P1205)

### 思路：
纯纯史山大模拟，注意细节即可。

### 代码(c++)：
时间复杂度 **O(n^2)**  

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
char a[15][15], b[15][15], c[15][15], d[15][15];

// 90度顺时针旋转
bool rotate90() {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            b[j][n - i + 1] = a[i][j];
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (b[i][j] != c[i][j]) return false;
    return true;
}

// 180度旋转
bool rotate180() {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            b[n - i + 1][n - j + 1] = a[i][j];
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (b[i][j] != c[i][j]) return false;
    return true;
}

// 270度旋转
bool rotate270() {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            b[n - j + 1][i] = a[i][j];
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (b[i][j] != c[i][j]) return false;
    return true;
}

// 水平翻转
bool reflect() {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            b[i][n - j + 1] = a[i][j];
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (b[i][j] != c[i][j]) return false;
    return true;
}

// 水平翻转后再旋转
bool reflectAndRotate() {
    reflect();
    // 复制翻转后的b到a
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            a[i][j] = b[i][j];
    if (rotate90()) return true;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            a[i][j] = b[i][j];
    if (rotate180()) return true;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            a[i][j] = b[i][j];
    if (rotate270()) return true;
    return false;
}

// 不变
bool noChange() {
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (a[i][j] != c[i][j]) return false;
    return true;
}

void solve() {
    if (rotate90()) { cout << 1; return; }
    if (rotate180()) { cout << 2; return; }
    if (rotate270()) { cout << 3; return; }
    if (reflect()) { cout << 4; return; }
    if (reflectAndRotate()) { cout << 5; return; }
    if (noChange()) { cout << 6; return; }
    cout << 7;
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++) {
            cin >> a[i][j];
            d[i][j] = a[i][j];
        }
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            cin >> c[i][j];
    solve();
    return 0;
}
```

