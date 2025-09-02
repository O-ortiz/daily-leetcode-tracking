Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

**Input:** nums = [1,2,3,1]

**Output:** true

**Explanation:**

The element 1 occurs at the indices 0 and 3.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** false

**Explanation:**

All elements are distinct.

**Example 3:**

**Input:** nums = [1,1,1,3,3,4,3,2,4,2]

**Output:** true

**Constraints:**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`


---

Hooray we can finally use a hash table again. This should be a similar solution to one of our solutions, of all things, Linked List Cycle. We can make a hash table, that adds every number to the hash as a key. If they number is already there, immediately return true. In the worst case this will run in O(n) since hash look up is in O(1). 

This problem is also tagged "Sorting" and that makes sense because we can sort the array and THEN traverse it, but I feel like on average that is worse since the sorting is O(n ln(g)) and just traversing and adding/checking a hash is only O(n). I could be wrong though. 

```
class Solution {

public:

    bool containsDuplicate(vector<int>& nums) {
		std::unordered_map<int,int> hash;
		vector<int>::iterator it;
		
		for(it = nums.begin(); it != nums.end() )
    }

};
```
