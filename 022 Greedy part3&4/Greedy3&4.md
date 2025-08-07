# 贪心算法笔记汇总（2025-08-07）

### 134. 加油站

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int cursum = 0;
        int totalsum = 0;
        int start = 0;

        for(int i=0; i<gas.size(); i++){
            cursum += gas[i] - cost[i];
            totalsum += gas[i] - cost[i];
            if(cursum < 0){
                start = i + 1;
                cursum = 0;
            }
        }
        if(totalsum < 0) return -1;
        return start;
    }
};
```

- 思路：局部油量小于 0 时重置起点，全局总和为负则无解。

---

### 135. 分发糖果

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);
        for(int i = 1; i < ratings.size(); i++){
            if(ratings[i] > ratings[i - 1])
                candyVec[i] = candyVec[i - 1] + 1;
        }
        for(int i = ratings.size() - 2; i >= 0; i--){
            if(ratings[i] > ratings[i + 1]){
                candyVec[i] = max(candyVec[i], candyVec[i + 1] + 1);
            }
        }
        return accumulate(candyVec.begin(), candyVec.end(), 0);
    }
};
```

- 思路：两次遍历，分别从左往右和右往左处理递增关系。

---

### 860. 柠檬水找零

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0;
        for (int bill : bills) {
            if (bill == 5) five++;
            else if (bill == 10) {
                if (five == 0) return false;
                five--; ten++;
            } else {
                if (five > 0 && ten > 0) { five--; ten--; }
                else if (five >= 3) { five -= 3; }
                else return false;
            }
        }
        return true;
    }
};
```

- 思路：优先用 10 元找零，保留更多 5 元。

---

### 406. 根据身高重建队列

```cpp
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), cmp);
        vector<vector<int>> que;
        for (auto& p : people) {
            que.insert(que.begin() + p[1], p);
        }
        return que;
    }
};
```

- 思路：身高降序，按位置插入。

---

### 452. 用最少数量的箭引爆气球

```cpp
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }

public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.empty()) return 0;
        sort(points.begin(), points.end(), cmp);
        int result = 1;
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > points[i - 1][1]) result++;
            else points[i][1] = min(points[i][1], points[i - 1][1]);
        }
        return result;
    }
};
```

- 思路：每次尽量重叠多些区间，重叠部分变小。

---

### 435. 无重叠区间

```cpp
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }

public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() <= 1) return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int repeat = 0;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < intervals[i - 1][1]) {
                intervals[i][1] = min(intervals[i][1], intervals[i - 1][1]);
                repeat++;
            }
        }
        return repeat;
    }
};
```

- 思路：结束时间最早的保留，冲突区间丢弃。

---

### 763. 划分字母区间

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            hash[s[i] - 'a'] = i;
        }

        int left = 0, right = 0;
        vector<int> result;
        for (int i = 0; i < s.size(); i++) {
            right = max(right, hash[s[i] - 'a']);
            if (i == right) {
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
    }
};
```

- 思路：记录每个字母出现的最远位置，当扫描到最远位置即为一个片段。

---

> 📅 本文档记录了 2025-08-07 的所有贪心算法笔记和题解，适合复习与面试准备。建议搭配图解深入理解贪心策略的局限性和适用场景。
