A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given: a string `s`
Return: `true` _if it is a **palindrome**, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"
**Output:** true
**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**

**Input:** s = "race a car"
**Output:** false
**Explanation:** "raceacar" is not a palindrome.

**Example 3:**

**Input:** s = " "
**Output:** true
**Explanation:** s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.

**Constraints:**

- `1 <= s.length <= 2 * 105`
- `s` consists only of printable ASCII characters.


---

Once again I am tempted to use a hash-map. However, just like the valid parentheses problem, that misses many cases where the string might have even pairs of characters, but are not ordered like a palindrome. Oh and the case where it's a palindrome but has an odd number of characters. Oh and the string might also have alphanumeric characters. 

We could go through the string and strip any non alpha-numeric characters but then that would require traversing the array more than once. Which I don't wanna do, ya know?  

We can likely employ a two pointers solution to this problem and moving the ptr over if we encounter a non-alpha numeric object. In Python we could just use a very handy function that in near constant time removes all of the non alpha-numeric characters. But that's not how it works around here in the world of C++ so we just have to make do. We can just check in our iteration if a character our current prt index is non alpha-numerical and move it to the next character. We only need to do it for the case where pt1 < pt2, because if pt1 and pt2 are the same and it hasn't returned false yet then it must be a palindrome. 

Oh we also need to return true if it's an empty string since that's an edge case. And if s is a single character we also return true. Oh and we need to make sure that we compare the lowercase version of each character since the alpha-numerical value is what matters and the not the case. 

```
class Solution {

public:
    bool isPalindrome(string s) {
        if (s.size() <= 1){
            return true;
        };
        int pt1 = 0;
        int pt2 = s.size()-1;     
        while (pt1<pt2) {
            
            if(!std::isalnum(s[pt1])){
                pt1++;
                continue;
            }

            else if(!std::isalnum(s[pt2])){
                pt2--;
                continue;
            }
            else{
                if(std::tolower(s[pt1]) != std::tolower(s[pt2])){
                    return false;
				}
			}
			pt1++;
			pt2--;
        }
        
        return true; 
    }

};
```

This solution works and is O(n); Also, just realized the continues aren't necessary if I only increment the ptr values in the last else where they both are alpha numeric, which is better form. 

```
class Solution {

public:
    bool isPalindrome(string s) {
        if (s.size() <= 1){
            return true;
        };
        int pt1 = 0;
        int pt2 = s.size()-1;     
        while (pt1<pt2) {
            
            if(!std::isalnum(s[pt1])){
                pt1++;
            }

            else if(!std::isalnum(s[pt2])){
                pt2--;
            }
            else{
                if(std::tolower(s[pt1]) != std::tolower(s[pt2])){
                    return false;
				}
				pt1++;
				pt2--;
			}

        }
        
        return true; 
    }

};
```