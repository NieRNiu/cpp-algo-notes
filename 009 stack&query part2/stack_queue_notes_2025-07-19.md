# 栈与队列 - LeetCode 题解笔记（2025-07-19）

---

## 📌 LeetCode 150. 逆波兰表达式求值（Evaluate Reverse Polish Notation）

### 解题思路
使用栈模拟逆波兰表达式的计算过程：
- 遇到操作数就入栈；
- 遇到操作符就弹出两个数，进行计算，再将结果入栈；
- 最终栈中只剩一个值，即为表达式的结果。

### C++代码
```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long long> st;
        for(int i = 0; i < tokens.size(); i++){
            if(tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/"){
                long long num1 = st.top(); st.pop();
                long long num2 = st.top(); st.pop();
                if (tokens[i] == "+") st.push(num2 + num1);
                if (tokens[i] == "-") st.push(num2 - num1);
                if (tokens[i] == "*") st.push(num2 * num1);
                if (tokens[i] == "/") st.push(num2 / num1);
            } else {
                st.push(stoll(tokens[i]));
            }
        }
        return st.top();
    }
};
```

---

## 📌 LeetCode 239. 滑动窗口最大值（Sliding Window Maximum）

### 解题思路
使用单调队列：
- 每次插入一个新元素时，维护队列从大到小的顺序；
- 队首元素始终是当前窗口中的最大值；
- 滑动窗口左侧元素滑出时，如果刚好是队首，则从队列中移除。

### C++代码
```cpp
class Solution {
private:
    class MyQuery {
    public:
        deque<int> que;
        void pop(int value) {
            if(!que.empty()&&value==que.front()) que.pop_front();
        }
        void push(int value) {
            while(!que.empty()&&value>que.back()) que.pop_back();
            que.push_back(value);
        }
        int front() { return que.front(); }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQuery que;
        vector<int> result;

        for(int i = 0; i<k; i++) que.push(nums[i]);
        result.push_back(que.front());
        for(int i = k; i<nums.size(); i++) {
            que.pop(nums[i-k]);
            que.push(nums[i]);
            result.push_back(que.front());
        }
        return result;
    }
};
```

---

## 📌 LeetCode 347. 前 K 个高频元素（Top K Frequent Elements）

### 解题思路
1. 使用哈希表统计每个元素出现的次数；
2. 用小顶堆（按频率排序），只保留频率最高的 k 个元素；
3. 将堆中元素收集起来即为结果。

### C++代码
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freqMap;
        for (int num : nums) freqMap[num]++;

        using PII = pair<int, int>;
        auto cmp = [](const PII& a, const PII& b) {
            return a.second > b.second;
        };
        priority_queue<PII, vector<PII>, decltype(cmp)> minHeap(cmp);

        for (const auto& entry : freqMap) {
            minHeap.push(entry);
            if (minHeap.size() > k) minHeap.pop();
        }

        vector<int> result;
        while (!minHeap.empty()) {
            result.push_back(minHeap.top().first);
            minHeap.pop();
        }
        return result;
    }
};
```
