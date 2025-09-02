Given a positive integer n, write a function that returns the number of set bits in its binary representation (also known as the Hamming weight).

 

Example 1:

Input: n = 11

Output: 3

Explanation:

The input binary string 1011 has a total of three set bits.

Example 2:

Input: n = 128

Output: 1

Explanation:

The input binary string 10000000 has a total of one set bit.

Example 3:

Input: n = 2147483645

Output: 30

Explanation:

The input binary string 1111111111111111111111111111101 has a total of thirty set bits.

 
Constraints:

1 <= n <= 231 - 1
 

Follow up: If this function is called many times, how would you optimize it?


---

Right, so bit manipulation. This is important and sometime trips me up. 

Usually for these problems we want to use the various logic operations (&&, ||, |=, &=, etc) as well as bit shifting (>>, <<) to figure out a solution like a puzzle. 

For this problem, the naïve way to do this would be to use a loop where while the number is not zero, we '&' it with 1, and increases a counter if true. Then bit shift right by one.


```
class Solution {
public:
    int hammingWeight(int n) {
        int count = 0;
        while(n != 0){
	        if(n & 1){
		        count++;
	        }
	        n = n >> 1;
        }
        
        return count;
        
    }
};
```

this works and is O(n) where n is the number of bits in the given integer. 