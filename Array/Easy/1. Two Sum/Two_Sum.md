# 🧮 Two Sum

## 📝 1. Problem Statement

You are given an array of integers `nums` and an integer `target`.
Your task is to **find two distinct indices** `i` and `j` in the array such that:

```
nums[i] + nums[j] == target
```

Return the indices of the two numbers **as an array** `[i, j]`.

If no such pair exists, return an empty array.
Assume there is **exactly one valid solution**, and you **may not use the same element twice**.

---

## 🧩 2. Example Test Cases

### Example 1:

**Input:**

```
nums = [2, 7, 11, 15]
target = 9
```

**Output:**

```
[0, 1]
```

**Explanation:**

* `nums[0] + nums[1] = 2 + 7 = 9`
* Indices `0` and `1` satisfy the condition.

---

### Example 2:

**Input:**

```
nums = [3, 2, 4]
target = 6
```

**Output:**

```
[1, 2]
```

**Explanation:**

* `nums[1] + nums[2] = 2 + 4 = 6`
* Hence, return `[1, 2]`.

---

## 📏 3. Constraints

* `2 <= nums.length <= 10⁴`
* `-10⁹ <= nums[i] <= 10⁹`
* `-10⁹ <= target <= 10⁹`
* **Only one valid solution** exists.

---

## 🪜 4. Brute Force Approach

### 🔹 Logic:

Check **all possible pairs** of elements to find two numbers that add up to the target.

### 🔹 Algorithm:

1. Iterate over each element `nums[i]`.
2. For every element `nums[i]`, iterate over the remaining elements `nums[j]` where `j > i`.
3. If `nums[i] + nums[j] == target`, return `[i, j]`.
4. If no pair is found, return an empty array.

---

### 🔹 Pseudocode:

```
for i from 0 to n-1:
    for j from i+1 to n-1:
        if nums[i] + nums[j] == target:
            return [i, j]
return []
```

---

### 🔹 Dry Run (Example: nums = [2, 7, 11, 15], target = 9)

| i                 | j | nums[i] | nums[j] | Sum | Matches? |
| ----------------- | - | ------- | ------- | --- | -------- |
| 0                 | 1 | 2       | 7       | 9   | ✅ Yes    |
| → Return `[0, 1]` |   |         |         |     |          |

---

### 🔹 Complexity:

* **Time Complexity:** O(n²)
  (Double loop over array elements)
* **Space Complexity:** O(1)
  (No extra data structures used)

---

### 🔹 C++ Implementation:

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); ++i) {
            for (int j = i + 1; j < nums.size(); ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```

---

### 🔹 Java Implementation:

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{};
    }
}
```

---

## ⚡ 5. Optimal Approach (Using Hash Map)

### 🔹 Logic:

Instead of checking all pairs, we use a **hash map** to store each number’s index while traversing the array.

At each step, check if the **complement** (`target - nums[i]`) already exists in the map:

* If it does, return the pair of indices.
* If not, add the current number and its index to the map.

This allows us to find the solution in a **single pass**.

---

### 🔹 Algorithm:

1. Initialize an empty hash map `numIndexMap`.
2. For each index `i` from `0` to `n-1`:

   * Compute `complement = target - nums[i]`.
   * If `complement` exists in the map, return `[numIndexMap[complement], i]`.
   * Otherwise, store `nums[i]` in the map with its index.
3. If no pair found, return an empty array.

---

### 🔹 Pseudocode:

```
create empty map numIndexMap

for i from 0 to n-1:
    complement = target - nums[i]
    if complement in numIndexMap:
        return [numIndexMap[complement], i]
    numIndexMap[nums[i]] = i

return []
```

---

### 🔹 Dry Run (Example: nums = [2, 7, 11, 15], target = 9)

| Step | i | nums[i] | complement | Hash Map (num → index) | Found?               |
| ---- | - | ------- | ---------- | ---------------------- | -------------------- |
| 1    | 0 | 2       | 7          | {2:0}                  | No                   |
| 2    | 1 | 7       | 2          | {2:0, 7:1}             | ✅ Yes → return [0,1] |

---

### 🔹 Complexity:

* **Time Complexity:** O(n)
  (Single pass with O(1) hash lookup)
* **Space Complexity:** O(n)
  (Stores up to n elements in the map)

---

### 🔹 C++ Implementation:

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> numIndexMap;
        for (int i = 0; i < nums.size(); ++i) {
            int complement = target - nums[i];
            if (numIndexMap.find(complement) != numIndexMap.end()) {
                return {numIndexMap[complement], i};
            }
            numIndexMap[nums[i]] = i;
        }
        return {};
    }
};
```

---

### 🔹 Java Implementation:

```java
import java.util.*;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numIndexMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (numIndexMap.containsKey(complement)) {
                return new int[]{numIndexMap.get(complement), i};
            }
            numIndexMap.put(nums[i], i);
        }
        return new int[]{};
    }
}
```

---

## 🔍 6. Comparison

| Criteria             | Brute Force               | Optimal (Hash Map)                       |
| -------------------- | ------------------------- | ---------------------------------------- |
| **Logic**            | Checks all pairs manually | Uses lookup to find complement instantly |
| **Time Complexity**  | O(n²)                     | O(n)                                     |
| **Space Complexity** | O(1)                      | O(n)                                     |
| **Efficiency**       | Slow for large inputs     | Fast and scalable                        |
| **Readability**      | Simple but nested loops   | Slightly more advanced but cleaner       |

---

✅ **Final Verdict:**
Use the **Hash Map (Optimal)** approach — efficient, elegant, and easy to implement.
