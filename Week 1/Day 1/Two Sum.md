Given:  an array of integers `nums` and an integer `target
Return: _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?

---

I have done Two Sum with an ordered array before. That makes the solution simple since you can just implement a two pointer solution in O(ln(n)). 

Let's first consider the naïve case, that should take O(n^2). In this case we can check each index with every single other index, eventually finding the matching target. 

```
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		for (int i = 0; i < nums.size() -1; i++){
			for(int j = i + 1; j < nums.size(); j++){
				if (nums[i] + nums[j] == target){
					return {i,j};
				}
			}
		}
		return {};
	}
};
```

For the follow up, we can improve the algorithm by using a hash-map. A hashmap allows one to look up a value in O(1) time. That means that we can "cheat" a little using a simple trick. First we place each numbers as a key and their index as their value in the hashmap. For each item in the array, we can then search for its "corresponding" number in the hash. 

target - num1 = num2, by searching for num2 in the hash we can get the index of that number if it exits. We can only do this because each number is unique. 

```
class Solution {

public:
	vector<int> twoSum(vector<int>& nums, int target) {
		std::unordered_map<int, int> hash;
		for(int i = 0; i < nums.size(); i++){
			hash.insert({nums[i],i});
		}
		for(int i=0; i < nums.size(); i++){
			auto cor = hash.find(target - nums[i]);
			if(cor != hash.end() && i != cor->second){
				return {i, cor->second};
			}
		}
		return {};      
	}

};
```

I did have a bit of trouble with this soliton at first since I did not have the "&& i != cor->second" argument. This meant that inputs like [3,2,4] target 6 would return [0,0] since it would find that 6 -3 = 3, and then return the index of 3. So we needed to specifically check that it was not the same index as i in the loop. 

With this solution, the complexity if O(2n) so O(n) since we had to traverse the array twice. First to make the hash, and second to check if the corresponding number was in the hash. 

What if we checked the hash as we went along?

```
class Solution {

public:
	vector<int> twoSum(vector<int>& nums, int target) {
		std::unordered_map<int, int> hash;
		for(int i = 0; i < nums.size(); i++){
			hash.insert({nums[i],i});
			cor = hash.find(target-nums[i]);
			if(cor != hash.end() && i != cor->second){
				return {i, cor->second};
			}
			
		}
		return {};      
	}

};
```

With this solution the complexity is O(n) since we only traverse the array once. 
