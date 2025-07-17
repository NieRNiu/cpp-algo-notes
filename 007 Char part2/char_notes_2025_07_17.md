# 📘 字符串相关笔记（更新于 2025-07-17）

## 151. 反转字符串中的单词

### 📝 题目简述
给定一个字符串 `s`，请你反转字符串中单词的顺序。注意要去除多余的空格。

### 💡 解题思路
1. 先去除多余空格（包括前后空格、多个空格合并成一个）。
2. 将整个字符串整体反转。
3. 遍历字符串，逐个将每个单词进行反转。

### ✅ 代码实现
```cpp
class Solution {
public:
    void reverse(string&s, int start, int end){
        for(int i = start, j = end; i < j; i++,j--){
            swap(s[i],s[j]);
        }
    }

    void removeextraspace(string&s){
        int slow = 0;
        for(int i = 0; i < s.size(); ++i){
            if(s[i] != ' '){
                if (slow != 0) s[slow++] = ' ';
                while(i < s.size() && s[i] != ' '){
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow);
    }

    string reverseWords(string s) {
        removeextraspace(s);
        reverse(s, 0, s.size() - 1);

        int start = 0;
        for(int i = 0; i<= s.size(); ++i){
            if(i == s.size() || s[i] == ' '){
                reverse(s, start, i-1);
                start = i+1;
            }
        }
        return s;
    }
};
```

### 📊 复杂度分析
- 时间复杂度：O(n)
- 空间复杂度：O(1)

---

## 字符串右旋转

### 📝 题目简述
输入一个整数 `n` 和一个字符串 `s`，将字符串右旋 `n` 位。

### 💡 解题思路
字符串右旋其实是：
1. 先反转前半部分 `[0, len-n)`
2. 再反转后半部分 `[len-n, len)`
3. 最后整体反转

### ✅ 代码实现
```cpp
#include<iostream>
#include<algorithm>
using namespace std;

int main() {
    int n;
    string s;
    cin >> n;
    cin >> s;
    int len = s.size();
    reverse(s.begin(), s.begin() + len - n);
    reverse(s.begin() + len - n, s.end());
    reverse(s.begin(), s.end());
    cout << s << endl;
}
```

### 📊 复杂度分析
- 时间复杂度：O(n)
- 空间复杂度：O(1)

---

