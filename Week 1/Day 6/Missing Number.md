Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._

**Example 1:**

**Input:** nums = [3,0,1]

**Output:** 2

**Explanation:**

`n = 3` since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 2:**

**Input:** nums = [0,1]

**Output:** 2

**Explanation:**

`n = 2` since there are 2 numbers, so all numbers are in the range `[0,2]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 3:**

**Input:** nums = [9,6,4,2,3,5,7,0,1]

**Output:** 8

**Explanation:**

`n = 9` since there are 9 numbers, so all numbers are in the range `[0,9]`. 8 is the missing number in the range since it does not appear in `nums`.

---

Now this is interesting, we want to somehow figure out what number is missing in an array that has all of the numbers of a range from [0,n] except for one. The array is not in order. Sorting the array might make it easier, BUT this would mean a solution in O(n log n). 

I think we can find a solution in O(n) using a hash map or set. An ordered set takes O(log n) every time it inserts into it but we would still then need to go through the set which is O(n). So we would go through the given array in O(n), insert every time so it would be O(n *  log n), and then iterating through the set in O(n). Wait no this would just be the same as using a sort.

What if instead we iterated through the array, adding it to an unordered set. We know that the maximum number of the range should be the size of the array. We then iterate from 0 to nums.size() and check each number in the unordered set. This should be in O(n) since iterating through nums is O(n) and then checking each number in the unordered set is O(n). 

There is however, a solution that only uses O(1) space complexity (where an unordered set takes O(n) space complexity.) and O(n) time complexity. I just don't know what it is yet. Probably something to do with bit manipulation based off the tags. 

Lets implement the unordered set solution first and then try to figure out the one implied in the hint. 

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        std::unordered_set<int> uoset; 
        for (int num : nums){
		    uoset.insert(num);
	    }
	    
	    for (int i = 0; i <= nums.size(); i++){
		    if(uoset.find(i) == uoset.end()){
			    return i;
		    }
	    }
	    
	    return 0;
    }
};
```

Crazy, I got the solution in one try (except for a <= sign). Except it performed poorly in execution time and space complexity. 

Oh there is a SIMPLE solution that only uses O(1) space complexity that I completely overlooked. Just sum the total of what ALL the number from 0 to n would be, then iterate through nums and subtract from that total. The result should be the missing number. 

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int ans = 0;
        
		for (int i = 1; i <= nums.size(); i++){
		    ans += i;
	    }
        
        for (int num : nums){
		    ans -= num;
	    }
	    
	    
	    return ans;
    }
};
```

This solution is O(n) with O(1) space complexity. 

There IS another solution with bit manipulation, but I'm not gonna do all that. 