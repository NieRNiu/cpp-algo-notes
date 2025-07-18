# 栈与队列 · 笔记整理（2025-07-18）

## 理论基础：栈与队列

栈（Stack）：先进后出（LIFO）结构，常用于处理嵌套、递归等问题。
队列（Queue）：先进先出（FIFO）结构，适合处理任务调度、层序遍历等问题。

### STL 中的栈与队列（C++）

- `stack` 和 `queue` 并不是容器，而是容器适配器（container adapter）。
- 默认底层容器为 `deque`，也可指定 `vector`、`list`。
- 都不支持遍历和迭代器，封装性更强。

---

## LeetCode 232. Implement Queue using Stacks

用两个栈实现一个队列，`stIn` 负责入队，`stOut` 负责出队。

**思路：**
- 出队或取队首元素时，如果 `stOut` 为空，则将 `stIn` 元素倒入 `stOut`。
- 时间复杂度均摊为 O(1)。

```cpp
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;

    void push(int x) {
        stIn.push(x);
    }

    int pop() {
        if (stOut.empty()) {
            while (!stIn.empty()) {
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }

    int peek() {
        int res = this->pop();
        stOut.push(res);
        return res;
    }

    bool empty() {
        return stIn.empty() && stOut.empty();
    }
};
```

---

## LeetCode 225. Implement Stack using Queues

用两个队列实现一个栈，`que1` 主队列，`que2` 辅助队列。

**思路：**
- 每次弹出或获取栈顶元素时，将前 n-1 个元素转入辅助队列。
- 时间复杂度：`push` O(1)，`pop` 和 `top` O(n)

```cpp
class MyStack {
public:
    queue<int> que1;
    queue<int> que2;

    void push(int x) {
        que1.push(x);
    }

    int pop() {
        int size = que1.size();
        size--;
        while (size--) {
            que2.push(que1.front());
            que1.pop();
        }
        int result = que1.front();
        que1.pop();
        que1 = que2;
        while (!que2.empty()) que2.pop();
        return result;
    }

    int top() {
        int size = que1.size();
        size--;
        while (size--) {
            que2.push(que1.front());
            que1.pop();
        }
        int result = que1.front();
        que2.push(que1.front());
        que1.pop();
        que1 = que2;
        while (!que2.empty()) que2.pop();
        return result;
    }

    bool empty() {
        return que1.empty();
    }
};
```

---

## LeetCode 20. Valid Parentheses

判断括号是否有效匹配。

**思路：**
- 用栈记录每个左括号对应的右括号。
- 遇到右括号时，检查是否匹配栈顶，不匹配则无效。

```cpp
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 != 0) return false;
        stack<char> st;
        for (char c : s) {
            if (c == '(') st.push(')');
            else if (c == '[') st.push(']');
            else if (c == '{') st.push('}');
            else if (st.empty() || c != st.top()) return false;
            else st.pop();
        }
        return st.empty();
    }
};
```

---

## LeetCode 1047. Remove All Adjacent Duplicates In String

删除相邻重复字符。

**思路：**
- 使用 string 模拟栈结构。
- 相邻字符重复则 pop，否则 push。

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string result;
        for (char c : s) {
            if (!result.empty() && result.back() == c) {
                result.pop_back();
            } else {
                result.push_back(c);
            }
        }
        return result;
    }
};
```

---

> 如需生成 PDF、导出 ZIP 等功能，可继续告诉我。
