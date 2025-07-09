
### Array Theory Basics

Arrays are a **contiguous memory block** storing elements of the same type, supporting O(1) random access. Insertions and deletions require shifting elements, so time complexity is O(n). In C++, 2D arrays are stored contiguously; in Java, 2D arrays are arrays of arrays and not contiguous.

Indices start from 0, and addresses are continuous for fast access. Deletion requires shifting subsequent elements forward.

Example in C++ (2D array):
```cpp
int array[2][3] = {{0, 1, 2}, {3, 4, 5}};
cout << &array[0][0] << " " << &array[0][1] << endl;
```
Example in Java:
```java
int[][] arr = {{1, 2}, {3, 4}};
System.out.println(arr[0]);
```

`vector` in C++ is implemented using a dynamic array (still contiguous memory). Arrays are good for frequent random access; linked lists are better for frequent insertions and deletions.

---

### LeetCode 704. Binary Search

Find the target value in a sorted array and return its index; otherwise, return -1. Time complexity: O(log n).

**Left-closed, right-closed interval [left, right]**
```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

**Left-closed, right-open interval [left, right)**
```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size();
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid;
    }
    return -1;
}
```

Variants include finding insertion points or boundaries (e.g., LC35, LC34). It's important to clarify interval definitions, update logic, and termination conditions during interviews.

---

### LeetCode 27. Remove Element

Remove all instances of `val` in-place and return the new length. Brute force: O(n²); two-pointer: O(n).

**Brute force (nested loops)**
```cpp
int removeElement(vector<int>& nums, int val) {
    int size = nums.size();
    for (int i = 0; i < size; ++i) {
        if (nums[i] == val) {
            for (int j = i + 1; j < size; ++j) {
                nums[j - 1] = nums[j];
            }
            --size;
            --i;
        }
    }
    return size;
}
```

**Two-pointer approach**
```cpp
int removeElement(vector<int>& nums, int val) {
    int slow = 0;
    for (int fast = 0; fast < nums.size(); ++fast) {
        if (nums[fast] != val) {
            nums[slow++] = nums[fast];
        }
    }
    return slow;
}
```

Related problem: LC26 Remove Duplicates from Sorted Array. Mastering in-place modification and two-pointer techniques is essential.

---

### LeetCode 977. Squares of a Sorted Array

Return a sorted array of squares of each number. Two-pointer approach, O(n).

Compare absolute values from both ends and fill the result array from the back.
```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    vector<int> res(nums.size());
    int pos = nums.size() - 1;
    while (left <= right) {
        if (abs(nums[left]) > abs(nums[right])) {
            res[pos--] = nums[left] * nums[left];
            ++left;
        } else {
            res[pos--] = nums[right] * nums[right];
            --right;
        }
    }
    return res;
}
```

Similar problems include merging two sorted arrays and sorting by absolute value. Mastering two-pointer patterns is crucial for array problems.

---

### Reference
[Array] https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html

[704] https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html

[27] https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html

[977] https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html




