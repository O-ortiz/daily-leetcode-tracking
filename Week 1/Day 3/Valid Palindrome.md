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

Once again I am tempted to use a hash-map. However, just like the valid parentheses problem, that misses many cases where the string might have even pairs of characters, but are not ordered like a palindrome. Oh and the case where it's a palindrome but has an off number of characters. Oh and the string might also have alphanumeric characters. 

We could go through the string and strip any non alpha-numeric characters but then that would require traversing the array more than once. Which I don't wanna do, ya know?  

We can likely employ a two pointers solution to this problem and moving the ptr over if we encounter a non-alpha numeric object. In Python we could just use a very handy function that in near constant time removes all of the non alpha-numeric characters. But that's not how it works around here in the world of C++ so we just have to make do. We can just check in our iteration if a character our current prt index is non alpha-numerical and move it to the next character. 

Oh we also need to return true if it's an empty string since that's an edge case. 

```
class Solution {

public:

    bool isPalindrome(string s) {
        if (!s){ return true;}

  

    }

};
```