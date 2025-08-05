# 贪心算法总结（二）

## 核心思想

贪心算法的核心在于：**每一步都选择当前看起来最优的解（局部最优）**，最终希望能够得到全局最优解。

但贪心法不一定都能保证全局最优解，因此使用前要进行**可行性分析**。

典型特征：
- 可以“局部做决策”，不依赖后续选择。
- 题目中常常带有“最小/最大”、“最多/最少”、“最优”等字样。

---

## LeetCode 122. 买卖股票的最佳时机 II

**题目要求**：可以多次买卖，求最大收益。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        if(prices.size() <= 1) return 0;
        for(int i=1; i<prices.size(); i++){
            result += max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
};
```

**解题思路**：
- 把所有“上涨的区间”利润都加起来。
- 局部最优是：只要今天比昨天贵，就昨天买今天卖。

---

## LeetCode 55. 跳跃游戏

**题目要求**：从数组起点出发，看是否能跳到终点。

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        if(nums.size() == 1) return true;
        int cover = 0;
        for(int i = 0; i <= cover; i++){
            cover = max(i + nums[i], cover);
            if(cover >= nums.size() - 1) return true;
        }
        return false;
    }
};
```

**解题思路**：
- 每次更新当前可以覆盖的最远位置 `cover`。
- 如果某个位置 i > cover，说明无法到达，返回 false。
- 如果在过程中 `cover >= nums.size() - 1`，说明可以到达终点。

---

## LeetCode 45. 跳跃游戏 II

**题目要求**：求从起点跳到终点所需的最小跳跃次数。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int cur = 0;
        int next = 0;
        int ans = 0;
        if(nums.size() == 1) return 0;
        for(int i = 0; i < nums.size() - 1; i++){
            next = max(nums[i] + i, next);
            if(i == cur){
                cur = next;
                ans++;
            }
        }
        return ans;
    }
};
```

**解题思路**：
- 每一跳的区间是 `[i, cur]`，在其中找到下一次能跳最远的 `next`。
- 一旦跳出当前跳跃区间，则增加步数。

---

## LeetCode 1005. K 次取反后最大化的数组和

**题目要求**：最多进行 K 次取反操作，返回最大数组和。

```cpp
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& A, int K) {
        sort(A.begin(), A.end(), cmp);       // 第一步：按绝对值降序排序
        for (int i = 0; i < A.size(); i++) { // 第二步：优先处理负数
            if (A[i] < 0 && K > 0) {
                A[i] *= -1;
                K--;
            }
        }
        if (K % 2 == 1) A[A.size() - 1] *= -1; // 第三步：如果还有奇数次取反，反转最小的数
        int result = 0;
        for (int a : A) result += a;        // 第四步：求和
        return result;
    }
};
```

**解题思路**：
- 贪心策略：总是选择当前绝对值最大的元素进行操作。
- 优先反转负数，若 K 有剩余则取反最小的正数。

---
