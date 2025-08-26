Given: a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`
Return: true if the input string is valid, false otherwise.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "([])"

**Output:** true

**Example 5:**

**Input:** s = "([)]"

**Output:** false

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.


---

To solve this, we need to traverse the characters in string s. We can keep a stack of characters to check if the solution is valid. 

For example is the string is " (())", we can first take the first character "(" and add it to the stack. We then look at the second character. "(". If it is not the corresponding pair for the item currently at the top of the stack, then we add it to the stack as well. The third character is ")", so we can check the top of the stack. Since "(" is the matching pair of ")" we can pop it from the stack. 

If we arrive to the end of the string s and there are items in the stack, then we know that the string is not valid. 

```
class Solution {
public:
    bool isValid(string s) {
        std::stack<char> aStack;
        
		for (char c : s){
			if(!aStack.empty()){
                if((aStack.top() == '(' && c == ')') || (aStack.top()  == '[' && c == ']') || (aStack.top()  == '{' && c == '}')){
					aStack.pop();
				}
			}
			
			if(c  == '(' || c  == '[' || c == '{'){
				aStack.push(c);
			}
		}
		
		if (aStack.empty()){
			return true;
		}
		return false;
    }

};
```

This should be O(n), but there are a some edge cases we need to address. For example, for an input like }}}. 

I think we can probably implement a hash map solution for this problem as well. 

```
class Solution {
public:
    bool isValid(string s) {
        std::unordered_map<char, int> hash;
		for (char c: s){
			hash[c]++;
		}
		
		if (hash['('] == hash[')'] && hash['{'] == hash['}'] && hash['['] == hash[']']){
		return true;
		}
		return false;
 
    }
	
};
```

This implementation has the issue that it cannot detect "][}{" for example. Sure there are enough of each bracket, but this string is not actually valid. 

What if we combined our solutions while trying to maintain complexity. 

```
class Solution {
public:
    bool isValid(string s) {
        std::unordered_map<char, int> hash;
        std::stack<char> stack;
		for (char c: s){
			hash[c]++;

			if(!stack.empty()){
                if((stack.top() == '(' && c == ')') || (stack.top()  == '[' && c == ']') || (stack.top()  == '{' && c == '}')){
                    stack.pop();
				}
			}
			
            if(c  == '(' || c  == '[' || c == '{'){
				stack.push(c);
            }
		}
		
		if (hash['('] == hash[')'] && hash['{'] == hash['}'] && hash['['] == hash[']'] && stack.empty()){
		    return true;
		}
		return false;
 
    }
	
};
```

This solutions combines our hash-map and stack solution,  keeping track of unpaired brackets as well as detecting inputs the stack solution alone couldn't. 

I suspect there is a solution using just the stack that identifies when there is a left bracket in the stack and immediately returns false. Modifying our original stack solution gives us:

```
class Solution {
public:
    bool isValid(string s) {
        std::stack<char> aStack;
		for (char c : s){
			if(!aStack.empty()){
                if(aStack.top()  == ')' || aStack.top()  == ']' || aStack.top() == '}'){
				    return false;
			    }
                if((aStack.top() == '(' && c == ')') || (aStack.top()  == '[' && c == ']') || (aStack.top()  == '{' && c == '}')){
					aStack.pop();
                    continue;
				}
                
			}	
			aStack.push(c);
		}
        return aStack.empty();
    }

};
```

This is O(n) since it only iterates through each character in s once and passes all tests using only stacks. The important part of this implementation is "continue" which ensures that a right side bracket isn't pushed into the stack if it is popping a left side bracket from the stack. 

