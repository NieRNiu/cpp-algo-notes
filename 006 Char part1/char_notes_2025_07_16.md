#### LeetCode 344. Reverse String

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int length = s.size();
        for(int i = 0, j = length - 1; i < j; i++, j--){
            swap(s[i], s[j]);
        }
    }
};
```

---

#### 字符处理专题笔记（2025-07-16）

---

### Leetcode 541. 反转字符串 II

**题目描述：**
给定一个字符串 `s` 和一个整数 `k`，你需要对每个 2k 个字符的前 k 个字符进行反转。

**代码实现：**
```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i= 0; i<s.size();i+=2*k){
            if(i+k<=s.size()){
                reverse(s.begin()+i,s.begin()+i+k);
            }
            else{
                reverse(s.begin()+i,s.end());
            }
        }
        return s;
    }
};
```

**知识点拓展：**
- `reverse(s.begin(), s.end())` 是 STL 中反转函数。
- 控制每 2k 个字符的片段起点为 `i`，通过判断剩余字符长度来决定反转范围。

---

### 字符串中替换数字为 "number"

**功能描述：**
将字符串中的所有数字（0-9）替换成 `"number"`。

**代码实现：**
```cpp
#include<iostream>
using namespace std;

int main(){
    string s;
    while(cin>>s){
        int oldsize = s.size() - 1;
        int count = 0;
        for(int i = 0; i < s.size(); i++){
            if(s[i] >= '0' && s[i] <= '9'){
                count++;
            }
        }

        s.resize(s.size() + count * 5);
        int newsize = s.size() - 1;

        while(oldsize>=0){
            if(s[oldsize] >= '0' && s[oldsize] <= '9' ){
                s[newsize--] = 'r';
                s[newsize--] = 'e';
                s[newsize--] = 'b';
                s[newsize--] = 'm';
                s[newsize--] = 'u';
                s[newsize--] = 'n';
            }
            else{
                s[newsize--] = s[oldsize];
            }
            oldsize--;
        }

    cout << s << endl;
    }
}
```

**知识点拓展：**
- 替换时先计算扩展后的字符串总长度，避免边遍历边扩容。
- 使用双指针从末尾向前写入，防止数据覆盖。
- 字符替换操作常用于 URL 编码、日志脱敏等场景。

---

> 今日重点：理解字符串处理的「原地修改」技巧，掌握反转与替换两类经典操作。
