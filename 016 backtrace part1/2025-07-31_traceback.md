# 2025-07-31 回溯法总结笔记

今日整理回溯法的基本思想与三道典型题目：组合（77）、组合总和III（216）、电话号码的字母组合（17）。

---

## 回溯法核心思想

1. **路径**：当前已选择的内容，用于保存阶段性结果。
2. **选择列表**：当前位置可以进行的所有选择（通常通过循环遍历）。
3. **结束条件**：递归的退出条件，一旦达到目标就保存结果或返回。
4. **回溯**：递归返回后，撤销上一步的选择（`pop_back` 或其他操作）以便尝试下一种可能。

回溯模板：

```cpp
void backtrack(参数...) {
    if (满足结束条件) {
        记录结果;
        return;
    }
    for (选择 in 选择列表) {
        做选择;
        backtrack(...);
        撤销选择;
    }
}
```

回溯的本质就是「穷举 + 剪枝」。

---

## 77. 组合

### 代码：
```cpp
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;
    void backtrack(int n, int k, int startidx){
        if(path.size()==k){
            result.push_back(path);
            return;
        }
        for(int i = startidx; i <= n; i++){
            path.push_back(i);
            backtrack(n, k, i+1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        result.clear();
        path.clear();
        backtrack(n, k, 1);
        return result;      
    }
};
```

**思路说明**：依次选择下一个数字，递归生成所有长度为 k 的组合。

---

## 216. 组合总和 III

### 代码：
```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int targetSum, int k, int sum, int startIndex) {
        if (path.size() == k) {
            if (sum == targetSum) result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 9; i++) {
            sum += i;
            path.push_back(i);
            backtracking(targetSum, k, sum, i + 1);
            sum -= i;
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear();
        path.clear();
        backtracking(n, k, 0, 1);
        return result;
    }
};
```

**思路说明**：在组合的基础上加入和的约束，终止条件需要检查 `sum == targetSum`。

---

## 17. 电话号码的字母组合

### 代码：
```cpp
class Solution {
private:
    const string letterMap[10] = {
        "", "", "abc", "def", "ghi", "jkl",
        "mno", "pqrs", "tuv", "wxyz",
    };
public:
    vector<string> result;
    string s;
    void backtracking(const string& digits, int index) {
        if (index == digits.size()) {
            result.push_back(s);
            return;
        }
        int digit = digits[index] - '0';
        string letters = letterMap[digit];
        for (int i = 0; i < letters.size(); i++) {
            s.push_back(letters[i]);
            backtracking(digits, index + 1);
            s.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        s.clear();
        result.clear();
        if (digits.size() == 0) {
            return result;
        }
        backtracking(digits, 0);
        return result;
    }
};
```

**思路说明**：回溯逐位处理输入的数字，每一位数字对应多个字母，形成一棵决策树，所有路径组成最终答案。

---

这些题目展示了回溯法解决**组合类问题**的基本方法。
