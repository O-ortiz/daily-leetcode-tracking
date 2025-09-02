Reverse bits of a given 32 bits signed integer.

**Example 1:**

**Input:** n = 43261596

**Output:** 964176192

**Explanation:**

|Integer|Binary|
|---|---|
|43261596|00000010100101000001111010011100|
|964176192|00111001011110000010100101000000|

**Example 2:**

**Input:** n = 2147483644

**Output:** 1073741822

**Explanation:**

|Integer|Binary|
|---|---|
|2147483644|01111111111111111111111111111100|
|1073741822|00111111111111111111111111111110|

**Constraints:**

- `0 <= n <= 231 - 2`
- `n` is even.

**Follow up:** If this function is called many times, how would you optimize it?

---
Right, for this case we want to swap all the binary values from one end of int to the other end. 

We can use std::bitset to access an bit position directly, which is really handy. We then use a for loop to swap the bits from one end with the others. 

```
class Solution {
public:
    int reverseBits(int n) {
        std::bitset<32> aBitset(n);

		for(int i =0; i < 16; i++){
				auto temp = aBitset.test(i);
				aBitset[i] = aBitset[31-i];
				aBitset[31-i] = temp;
		}

		return (int)aBitset.to_ulong();
    
    }
};


```

This could be completely wrong. Would this really work? Turns out it does and passes all tests, but not before I needed to debug it a little. 

One of the problems I encountered was with storing the temp value. I originally had temp = aBitset[i] but this stores the *reference* of the bit at that address. Meaning that when we then set aBitset[i] to aBitset[31-i], we changed the reference at index i to the reference at index 31-i. And we then set the reference at [31-i] to the reference from i. Which only actually flipped the first half of the int bits and not the second half. 

SO to fix this I used .test() since that returns the value of the bit and not the reference. 

This solution works and is O(n). I bet there's a better solution though. 

And there is a solution that ONLY uses bit shifting, because of course there is. Basically take a int result = 0, then iterate from 0 to 31. Take the value at that position of 0, bit shift it to the left 31-i times, then OR it with the result. Then shift n to the right by one. Eventually n will be zero once we have shifted it to the right 32 times.  

```
class Solution {
public:
    int reverseBits(int n) {
        int result;
        for(int i = 0; i < 32; i++){ 
	        result |= (n & 1) << (31 - i); 
            n = n>> 1;
        }
        return result;
    }
};
```

