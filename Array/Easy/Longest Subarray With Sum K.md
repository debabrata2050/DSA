# Longest Subarray with sum K 
Given an array `a[]` containing integers and an integer `k`, your task is to find the length of the longest subarray where the sum of its elements is equal to the given value `k`. If there is no subarray with sum equal to `k`, return 0.

Constraints:  
1 <= n <= 5 * 10 ^ 6  
1 <= k <= 10^18  
0 <= a[i] <= 10 ^ 9  

## 🧪 Dry Run
### Input:
```
a = [2, 1, 0, 1, 1]
k = 3
```
> 👉 Notice the 0 — this is the key to understanding why we only store the first occurrence of a prefix sum.

### Expected Output: 4

### Visulation:
```
Index:   0   1   2   3   4
Array:   2   1   0   1   1
Sum:     2   3   3   4   5
```

Init:
```
mp = {}
maxLen = 0
sum = 0
```

i = 0:
```
sum = 2
sum-k = -1 → not found

mp = {2: 0}
```

i = 1:
```
sum = 3
sum == k → maxLen = 2
sum-k = 0 → not found

mp = {2: 0, 3: 1}
```

⚠️ i = 2:
```
sum = 3
sum == k → maxLen = 3
sum-k = 0 → not found
3 already exists → DO NOT update

mp = {2: 0, 3: 1}
```
> 👉 Keep earliest index for longer future subarrays

i = 3:
```
sum = 4
sum-k = 1 → not found

mp = {2: 0, 3: 1, 4: 3}
```

i = 4:
```
sum = 5
sum-k = 2 → found at index 0
length = 4 - 0 = 4
maxLen = 4

mp = {2: 0, 3: 1, 4: 3, 5: 4}
```

### Code:
```java
import java.util.HashMap;

public class Solution {
    public static int longestSubarrayWithSumK(int []a, long k) {
        HashMap<Long, Integer> mp = new HashMap<>();
        int maxLen = 0;
        long sum = 0;
        for(int i=0; i < a.length; ++i){
            sum += a[i];
            // Check if sum equals k
            if(sum == k){
                maxLen = Math.max(maxLen, i+1);
            }
            // Check if (sum - k) exists in the map
            if(mp.containsKey(sum-k)){
                maxLen = Math.max(maxLen, i-mp.get(sum-k));
            }
            // We need the leftmost index for a prefix sum to maximize the length that's why we are not updating the map if the prefix sum already exists
            if(!mp.containsKey(sum)){
                mp.put(sum, i);
            }

            // You can also use the below line instead of the above if condition
            // mp.putIfAbsent(sum, i);
        }
        
        return maxLen;
    }
}
```

Notes:  
* https://www.youtube.com/watch?v=frf7qxiN2qU
* https://www.geeksforgeeks.org/dsa/longest-sub-array-sum-k/


Solve:  
* https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_6682399
* https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1