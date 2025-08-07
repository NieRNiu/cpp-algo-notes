# 贪心算法笔记（2025-08-07）


## ✅ 56. 合并区间（Merge Intervals）

### ✨ 题目描述

给出一组区间，合并所有重叠的区间。

### 🧠 解题思路

1. 按区间的起点进行升序排序。
2. 遍历排序后的区间数组：
   - 如果当前区间与结果中最后一个区间重叠，则更新结果区间的右边界。
   - 否则，将当前区间加入结果中。
3. 返回结果。

### 🧾 C++ 代码实现

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if (intervals.empty()) return result;

        sort(intervals.begin(), intervals.end(), 
             [](const vector<int>& a, const vector<int>& b) { return a[0] < b[0]; });

        result.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); ++i) {
            if (result.back()[1] >= intervals[i][0]) {
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            } else {
                result.push_back(intervals[i]);
            }
        }
        return result;
    }
};
```

### 🧩 示例

输入：`[[1,3],[2,6],[8,10],[15,18]]`  
输出：`[[1,6],[8,10],[15,18]]`

---

## ✅ 738. 单调递增的数字（Monotone Increasing Digits）

### ✨ 题目描述

给定一个非负整数 `N`，找出小于或等于 `N` 的最大数字 `X`，要求 `X` 的每位数字都是单调递增的（即从左到右每一位数字不减）。

### 🧠 解题思路

1. 将数字转为字符串，方便逐位操作。
2. 从右往左扫描，找到第一个逆序对（前一位比后一位大）。
3. 将前一位减 1，并记录需要变成 '9' 的起始位置。
4. 最后将记录位置之后的所有位都变成 '9'。
5. 转回整数返回。

### 🧾 C++ 代码实现

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string strnum = to_string(n);
        int flag = strnum.size();
        for (int i = strnum.size() - 1; i > 0; --i) {
            if (strnum[i] < strnum[i - 1]) {
                flag = i;
                strnum[i - 1]--;
            }
        }
        for (int i = flag; i < strnum.size(); ++i) {
            strnum[i] = '9';
        }
        return stoi(strnum);
    }
};
```

### 🧩 示例

输入：`n = 1234`  
输出：`1234`

输入：`n = 332`  
输出：`299`

---



