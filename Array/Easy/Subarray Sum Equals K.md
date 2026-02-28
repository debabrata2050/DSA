# Subarray Sum Equals K

Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

A subarray is a contiguous non-empty sequence of elements within an array.

Constraints:
1 <= nums.length <= 2 * 10^4
-1000 <= nums[i] <= 1000
-10^7 <= k <= 10^7

## Prefix Sum + HashMap Approach

Time Complexity: O(n)
Space Complexity: O(n)

### Algorithm

1. Initialize `count = 0` to keep track of the total number of valid subarrays.
2. Initialize `sum = 0` to store the running prefix sum.
3. Create a HashMap `map` to store the frequency of each prefix sum observed: `(prefix_sum -> frequency)`.
4. **Base Case:** Initialize the map with `0` occurring `1` time (`map.put(0, 1)`). This handles the case where a valid subarray exactly matches `k` starting from index `0`.
5. Iterate through each element in the array `nums`:
   - Add the current element to `sum`.
   - Calculate the target needed to form a valid subarray: `target = sum - k`.
   - If `target` exists in the `map`, it means we have found one or more early prefix sums that can be chopped off to leave a subarray of sum `k`. Add the frequency of `target` to `count`.
   - Add the current `sum` to the `map`. If it already exists, increment its frequency.
6. Return `count`.

### Code Implementation

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int sum = 0;
        
        // HashMap to store (prefix_sum, frequency)
        Map<Integer, Integer> map = new HashMap<>();
        
        // Base case: A prefix sum of 0 occurs 1 time (empty subarray)
        map.put(0, 1);
        
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i]; // Calculate current prefix sum
            
            // If (sum - k) is present in map, it means there is a valid subarray ending at this index
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            
            // Add the current prefix sum to the map for future subarray checks
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        
        return count;
    }
}
```

### Dry Run Example

**Input:** `nums = [10, 2, -2, -20, 10]`, `k = -10`

We start with a map containing `{0: 1}` (meaning a prefix sum of 0 has appeared 1 time).

| Step | Element | Running Sum | Target to look for in map<br>`(Running Sum - k)` | Was it in map? | Total Subarrays Found | Map after step |
|:---:|:---:|:---:|:---|:---|:---:|:---|
| 1 | `10` | **10** | `10 - (-10) = 20` | ❌ No | 0 | `{0:1, 10:1}` |
| 2 | `2`  | **12** | `12 - (-10) = 22` | ❌ No | 0 | `{0:1, 10:1, 12:1}` |
| 3 | `-2` | **10** | `10 - (-10) = 20` | ❌ No | 0 | `{0:1, 10:2, 12:1}` <br>*(Note: 10 appeared again)* |
| 4 | `-20`| **-10** | `-10 - (-10) = 0` | ✅ **Yes!** (0 appears 1 time) | `0 + 1 = 1` | `{0:1, 10:2, 12:1, -10:1}` |
| 5 | `10` | **0** | `0 - (-10) = 10` | ✅ **Yes!** (10 appears 2 times) | `1 + 2 = 3` | `{0:2, 10:2, 12:1, -10:1}` |

**Result:** `3`