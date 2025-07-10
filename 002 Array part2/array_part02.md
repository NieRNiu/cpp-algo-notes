### Array part02

### LeetCode 209. Minimum Size Subarray Sum

Given an array of positive integers `nums` and an integer `target`, find the minimal length of a contiguous subarray such that the sum ≥ `target`. If no such subarray exists, return 0.

Time complexity: O(n), using **sliding window**.

**Sliding Window Approach (with detailed C++ code and comments)**
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int result = INT32_MAX;
        int sum = 0;        // Current window sum
        int i = 0;          // Left boundary of the window
        int subLength = 0;  // Current window length

        for (int j = i; j < nums.size(); j++) {
            sum += nums[j];
            while (sum >= target) {
                subLength = j - i + 1;
                result = result < subLength ? result : subLength;
                sum -= nums[i++];  // Shrink window from the left
            }
        }
        return result != INT32_MAX ? result : 0;
    }
};
```

This code clearly shows:
- Expanding the window to the right by moving `j`.
- Shrinking the window from the left by moving `i` when the sum is enough.
- Updating the result whenever a valid window is found.

---

### LeetCode 59. Spiral Matrix II

Given an integer `n`, generate an `n x n` matrix filled with elements from 1 to n² in spiral order.

Time complexity: O(n²).

**Approach (C++ code with comments)**
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));
        int startx = 0, starty = 0; // Starting coordinates for each loop
        int loop = n / 2;           // Number of complete layers (loops)
        int mid = n / 2;            // Middle index for odd n
        int count = 1;              // Current number to fill
        int offset = 1;             // Controls boundary shrinking
        int i, j;

        while (loop--) {
            i = startx;
            j = starty;

            // Left to right
            for (j; j < n - offset; j++) {
                res[startx][j] = count++;
            }

            // Top to bottom
            for (i; i < n - offset; i++) {
                res[i][j] = count++;
            }

            // Right to left
            for (j; j > startx; j--) {
                res[i][j] = count++;
            }

            // Bottom to top
            for (i; i > starty; i--) {
                res[i][j] = count++;
            }

            startx++;
            starty++;
            offset++;
        }

        // Handle center element if n is odd
        if (n % 2) {
            res[mid][mid] = count++;
        }

        return res;
    }
};
```

Key points:
- Use `loop` to control how many layers to fill.
- Use `startx`, `starty`, and `offset` to track each layer’s bounds.
- Handle the middle element separately when `n` is odd.

---

### Reference
- [Programmer Carl - 209 Minimum Size Subarray Sum] https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html

- [Programmer Carl - 59 Spiral Matrix II] https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html

