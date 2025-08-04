# 贪心算法总结（2025-08-04）

## 理论基础

贪心算法（Greedy Algorithm）是一种在每一步选择中都采取当前状态下最优解的策略，以期通过局部最优达到全局最优。

它适用于具有 **贪心选择性质** 和 **最优子结构** 的问题，常见应用场景包括区间调度、最小生成树、最短路径、背包问题（部分）等。

贪心算法一般步骤为：

1. 将问题按某种规则排序。
2. 依次做出选择，保证局部最优。
3. 若能得到全局最优解，则问题适合贪心。

---

## LeetCode 455 分发饼干

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(), s.end());
        int index = s.size() - 1;
        int result = 0;
        for(int i = g.size() - 1; i >= 0; i--){
            if(index >= 0 && s[index]>=g[i]){
                result++;
                index--;
            }
        }
        return result;        
    }
};
```

### 思路解析

- 将孩子的胃口和饼干尺寸都排序；
- 从大胃口开始，尽量给最大的饼干；
- 能满足就分配，否则跳过；
- **贪心策略：给胃口大的孩子尽量分配大的饼干。**

---

## LeetCode 376 摆动序列

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()<=1) return 1;
        int prediff = 0;
        int curdiff = 0;
        int result = 1;
        for(int i = 0; i < nums.size() - 1; i++){
            curdiff = nums[i+1] - nums[i];
            if(prediff<=0&&curdiff>0 || prediff>=0&& curdiff<0){
                result++;
                prediff = curdiff;
            }
        }
        return result;
    }
};
```

### 思路解析

- 维护当前差值 `curdiff` 和上一次非零差值 `prediff`；
- 只要当前差值和前一次差值异号，就可以计入序列；
- **贪心策略：每次选择“拐点”加入序列，确保最长摆动子序列。**

---

## LeetCode 53 最大子序和

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
        int count = 0;
        for(int i=0; i< nums.size(); i++){
            count+=nums[i];
            if(count > result) result = count;
            if(count <= 0) count = 0;
        }
       return result; 
    }
};
```

### 思路解析

- 遍历过程中维护当前子序和；
- 如果当前子序和小于等于0，舍弃重来；
- **贪心策略：子序和为负则舍弃，保留对后面有利的子数组。**

---

## 总结

- 贪心不回退，效率高但不一定正确。
- 可用于最值类问题，但要验证是否满足贪心选择性质。
- 与动态规划对比：贪心更“局部”，DP更“全局”。

