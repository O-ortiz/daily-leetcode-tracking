
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
		
		for (int i = 0; i < nums.size(); i++){
			if(hash.find(nums[i])!=hash.end()){
				return true;
			}
			else{
		        hash.insert({nums[i], 1});
			}
		}
		
		return false;
    }

};
```

This feels a little weird since the hash value is just 1 every time, but that's literally all we need. We can actually instead of using a hash (unordered_map) instead use an unordered_set, which feel more intuitive:

```
class Solution {

public:

    bool containsDuplicate(vector<int>& nums) {
		std::unordered_set<int> seen;
		
		for (int x : nums){ // kinda forgot we can do this
			if(seen.count(x)){
				return true;
			}
			seen.insert(x);
		}
		
		return false;
    }

};
```

You know, I thought putting the array into a fancy function that can just sort the array for us was a Python thing only but turns out that no, we CAN use std::sort and just iterate through the resulting list and checking if the value to its right is the same. 

```
class Solution {

public:

    bool containsDuplicate(vector<int>& nums) {
		std::sort(nums.begin(),nums.end());
		
		for (int i=0; i < nums.size()-1; i++){ // kinda forgot we can do this
			if(nums[i] == nums[i+1]){
				return true;
			}
		}
		
		return false;
    }

};
```

The implementation with the hash/set is O(n). And the implementation with sort should be O(n lg n)


