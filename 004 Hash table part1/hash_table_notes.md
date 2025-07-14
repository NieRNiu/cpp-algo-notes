
### Hash Table Theory Basics

A **hash table** (also called a **hash map** or **dictionary**) is a data structure that directly accesses data via a **key** using a **hash function**.

#### What is a Hash Table?

A hash table maps keys to indices through a hash function, allowing for O(1) average-time complexity lookups.  

👉 In fact, an array can be viewed as a simple hash table where keys are index numbers.

A common use case is to quickly check whether an element exists in a set.  
- Without a hash table: O(n) time (linear scan).
- With a hash table: O(1) time (direct access).

#### Hash Function

The **hash function** converts a key (e.g., a string) into an integer (hash code), which is then mapped to a valid index in the hash table via modulus operation:

```
index = hashCode % tableSize
```

If the hash code is larger than `tableSize`, we use `%` to ensure the index falls within bounds.

#### Hash Collisions

When two keys hash to the same index, this is called a **hash collision**.

There are two common ways to handle collisions:

##### Separate Chaining

All elements that hash to the same index are stored in a linked list (or another structure) at that index.

##### Open Addressing (Linear Probing)

If a collision occurs, the algorithm searches linearly for the next available slot.  
Requires `tableSize > dataSize` to ensure free space.

#### Common Hash-based Structures

##### Set Structures in C++

| Structure           | Implementation | Ordered? | Duplicates | Modifiable keys | Query Time | Insert/Delete Time |
|----------------------|---------------|----------|-------------|----------------|-------------|-------------------|
| `std::set`          | Red-black tree | Yes      | No         | No             | O(log n)    | O(log n)         |
| `std::multiset`     | Red-black tree | Yes      | Yes        | No             | O(log n)    | O(log n)         |
| `std::unordered_set`| Hash table     | No       | No         | No             | O(1)        | O(1)             |

##### Map Structures in C++

| Structure             | Implementation | Ordered? | Duplicate keys | Modifiable keys | Query Time | Insert/Delete Time |
|------------------------|---------------|----------|----------------|----------------|-------------|-------------------|
| `std::map`           | Red-black tree | Yes      | No             | No             | O(log n)    | O(log n)         |
| `std::multimap`      | Red-black tree | Yes      | Yes            | No             | O(log n)    | O(log n)         |
| `std::unordered_map`| Hash table     | No       | No             | No             | O(1)        | O(1)             |

#### unordered_set vs hash_set

- `unordered_set` was introduced in C++11 as standard.
- `hash_set` and `hash_map` were pre-C++11 custom implementations and are now deprecated.

#### Summary

When you need to **quickly determine if an element exists**, consider using a hash-based approach. Hashing trades space for time by using extra storage for fast lookup.

---

### LeetCode 242. Valid Anagram

Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

**C++ Code**
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            record[s[i] - 'a']++;
        }
        for (int j = 0; j < t.size(); j++) {
            record[t[j] - 'a']--;
        }
        for (int k = 0; k < 26; k++) {
            if (record[k] != 0)
                return false;
        }
        return true;
    }
};
```

Key: Use a fixed-size array for fast counting.

---

### LeetCode 349. Intersection of Two Arrays

Return the intersection of two arrays; each element in the result must be unique.

**C++ Code**
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; 
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

Key: Use sets to ensure uniqueness and O(1) lookup.

---

### LeetCode 202. Happy Number

Determine whether a number is a "happy number".

**C++ Code**
```cpp
class Solution {
public:
    int getsum(int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n = n / 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        unordered_set<int> set;
        while (1) {
            int sum = getsum(n);
            if (sum == 1) {
                return true;
            } else if (set.find(sum) != set.end()) {
                return false;
            } else {
                set.insert(sum);
            }
            n = sum;
        }
    }
};
```

Key: Use a hash set to detect cycles.

---

### LeetCode 1. Two Sum

Return indices of the two numbers such that they add up to target.

**C++ Code**
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> idxMap;
        int n = nums.size();
        
        for (int i = 0; i < n; ++i) {
            int complement = target - nums[i];
            if (idxMap.find(complement) != idxMap.end()) {
                return { idxMap[complement], i };
            }
            idxMap[nums[i]] = i;
        }
        return {};
    }
};
```

Key: Use a hash map to achieve O(1) complement lookups.

---
