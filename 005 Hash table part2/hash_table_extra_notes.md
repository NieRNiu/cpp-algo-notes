
### When Hash Tables Are Not Suitable

Although hash tables are powerful for O(1) average-time lookups, they are **not always the best choice**, for example:

- When you need **ordered data** (e.g., ordered traversal or range queries).
- When keys or elements are **frequently modified**, as the hash value may change.
- When space is limited, because hash tables often require extra memory.

---

### Take Care of Duplicates

When using hash tables or arrays with two-pointer techniques, duplicates can easily occur if not handled carefully.

- Always **skip duplicates** during iteration, especially after sorting.
- Use a set if you need to guarantee uniqueness in the results.

---

### LeetCode 454. 4Sum II

Count tuples `(i, j, k, l)` such that:

```
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
```

**Approach**

- Use a hash map to store the sum of all pairs from `nums1` and `nums2` along with their frequencies.
- Iterate through all pairs in `nums3` and `nums4`, check if `-(c + d)` exists in the map.

**C++ Code**
```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> Map;
        int count = 0;
        for (int a : nums1) {
            for (int b : nums2) {
                Map[a + b]++;
            }
        }
        for (int c : nums3) {
            for (int d : nums4) {
                int target = 0 - (c + d);
                if (Map.find(target) != Map.end()) {
                    count += Map[target];
                }
            }
        }
        return count;
    }
};
```

Key: Reduce time complexity from O(n^4) to O(n^2) using hash map.

---

### LeetCode 15. 3Sum

Return all unique triplets `[nums[i], nums[j], nums[k]]` such that:

```
nums[i] + nums[j] + nums[k] == 0
```

**Approach**

- Sort the array.
- Iterate with index `i`, use two pointers `left` and `right` to find pairs.
- Skip duplicates for `i`, `left`, and `right` to ensure uniqueness.

**C++ Code**
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) break;
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < 0) left++;
                else if (sum > 0) right--;
                else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while (right > left && nums[right - 1] == nums[right]) right--;
                    while (right > left && nums[left + 1] == nums[left]) left++;
                    right--;
                    left++;
                }
            }
        }
        return result;
    }
};
```

Key: Carefully skip duplicates after finding valid triplets.

---

### LeetCode 18. 4Sum

Return all unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:

```
nums[a] + nums[b] + nums[c] + nums[d] == target
```

**Approach**

- Sort the array.
- Use nested loops to fix two numbers (`i`, `k`), and two pointers (`left`, `right`) to find the other two.
- Skip duplicates for all indices.

**C++ Code**
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0 && nums[i - 1] == nums[i]) continue;
            for (int k = i + 1; k < nums.size(); k++) {
                if (k > i + 1 && nums[k - 1] == nums[k]) continue;

                int left = k + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    long sum = (long)nums[i] + nums[k] + nums[left] + nums[right];
                    if (sum < target) left++;
                    else if (sum > target) right--;
                    else {
                        result.push_back(vector<int>{nums[i], nums[k], nums[left], nums[right]});
                        while (right > left && nums[left + 1] == nums[left]) left++;
                        while (right > left && nums[right - 1] == nums[right]) right--;
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```

Key: Use `long` type to avoid integer overflow when summing.

---
