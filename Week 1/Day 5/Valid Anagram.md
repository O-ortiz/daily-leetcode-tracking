[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)


Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

**Example 1:**

**Input:** s = "anagram", t = "nagaram"

**Output:** true

**Example 2:**

**Input:** s = "rat", t = "car"

**Output:** false

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?


---

Yay another hash map problem. Let's just think of the most naive solution and then try to find something better. 

We can start by making two unordered_maps to keep track of number of types of characters in each string. We can then compare the hash maps and if they don't have the same number of letters then we can just return false. 

Now I'm thinking this is overtly complicated. We can make use of the magical std::sort() again and sort both string alphabetically. Then we can just iterate through the string at the same time and immediately return false if they aren't the same length. 



```
class Solution {
public:
    bool isAnagram(string s, string t) {
		if(s.size() != t.size()){
			return false;
		}
        std::sort(s.begin(), s.end());
	    std::sort(t.begin(), t.end());   
		for(int i = 0; i< s.size(); i++){
			if(s[i] != t[i]){
				return false;
			}	
		}
		return true;
    }
};
```

This solution works, but also takes extra time due to std::sort, and then we iterate through so it's technically O(n + n lg n). 

If we want something a little more  optimal, we can instead of using a hash map or sorting the two strings, we can iterate through the two strings and keep a count of the number of 26 lowercase characters. We can do this because each cchar has a ASCII value, a = 65, b = 66, etc. This means that if we make an array of 26 ints, we can keep a count of each character in a string by doing character - 'a' which should place it in the corresponding location in the array. 

```
class Solution {
public:
    bool isAnagram(string s, string t) {
		if(s.size() != t.size()){
			return false;
		}
		std::array<int, 26> count1;
		std:array<int, 26> count2;
		
		for(int i = 0; i< s.size(); i++){
			count1[s[i] - 'a']++;
			count2[t[i] - 'a']++;
		}
		for(int i = 0; i < 26; i++){
			if(count1[i]!= count2[i]){
				return false;
			}
		}
		return true;
		
    }
};
```

This works and is O(n) since it just iterates though the strings once without needing to call any sort function. It did require some knowledge of ASCII characters though. 

If it was allowed to contain unicode characters, we would just have to use the hash map method since the trick with s[i] - 'a' cannot be employed for that. 