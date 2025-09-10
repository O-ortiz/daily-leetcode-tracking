Given an integer `n`, return _an array_ `ans` _of length_ `n + 1` _such that for each_ `i` (`0 <= i <= n`)_,_ `ans[i]` _is the **number of**_ `1`_**'s** in the binary representation of_ `i`.

**Example 1:**

**Input:** n = 2
**Output:** [0,1,1]
**Explanation:**
0 --> 0
1 --> 1
2 --> 10

**Example 2:**

**Input:** n = 5
**Output:** [0,1,1,2,1,2]
**Explanation:**
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101

**Constraints:**

- `0 <= n <= 105`

**Follow up:**

- It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
- Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

---

We already did a portion of this for number of 1 bits. Now we need to do this for all numbers from 0 to n. 


```
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> ans;

		for (int i = 0; i <= n ; i++){
			int count = 0;
			int cur = i;
	        while(cur != 0){
		        if(n & 1){
			        count++;
		        }
		        cur = cur >> 1;
	        }
	        
	        ans.push_back(count);
		
		}
        
        return ans;

    }
};
```

This solution unfortunately is n log n. Because we need to iterate through each number, so O(n) and for each number we need to find the number of 1 bits, which means iterating through each bit, which for each n is floor(log(n)) +1. Which means this solution means we iterate through O(n log n) things an is our runtime complexity. 

Okay, so we were told this solution would be easy, so let's try to figure out the more difficult solution. 

We can observe a pattern depending on if the number is even or odd. 

1 - > 01
3 -> 11
5-> 101
7->111



