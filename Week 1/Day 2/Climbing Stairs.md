You are climbing a staircase. It takes n steps to reach the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Given: n steps
Return: number of distinct ways you can climb up with the option of climbing one step at a time or two. 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45


---

 First let's start by identifying a pattern by continuing the example problems. 
 
 Input: n = 4
 Output: 5
 1. 1 step + 1 step + 1 step + 1 step
 2. 1 step + 1 step + 2 step
 3. 1 step + 2 step + 1 step
 4. 2 step + 1 step + 1 step
 5. 2 step + 2 step

Input: n = 5
Output:  8
1. 1 step + 1 step + 1 step + 1 step  + 1 step 
2. 1 step + 1 step + 1 step + 2 step
3. 1 step + 1 step + 2 step + 1 step
4. 1 step + 2 step + 1 step + 1 step
5. 2 step + 1 step + 1 step + 1 step
6. 1 step + 2 step + 2 step
7. 2 step + 1 step + 2 step
8. 2 step + 2 step + 1 step


While I feel like we can write a recurrence equation for this and solve it that way, the way this problem is written makes me think we can solve this using dynamic programming but I can't determine what we are trying to optimize or how to break this into subproblems. 

Solving this recursively would be far too impractical. 

As far as I can see, this looks like the Fibonacci sequence where the solution for f(n) = f(n-1) + f(n-2). We can see from our manual pattern of n = 5, the output f(5) = f(4) + f(3) = 5 + 3 = 8. 

Wait this kind of makes sense now as a dynamic problem solution because we can further elaborate f(4) = f(3) + f(2) = f(2) + f(1) + f(2). We know f(1) to just be 1, and f(2) to just be 2 so those are our base cases. 

If we solved this recursively it have to "unfurl" every single number down to the number of f(2) and f(1) functions that make up f(n). 

For f(4) = f(2) + f(2) + f(1) , this is two f(2) and one f(1)
For f(5) = f(2) + f(2) + f(1) + f(2) + f(1) , this is two f(2) and one f(1)

We can just use memorization using a dictionary where the key is the integer, and the value is the output. We can iterate up from n. If n is just 1 or two we can just return 1 or 2 respectively. 

If the value is n, we return f(n-1) + f(n-2). The helper function adds it to the dictionary as we build up to n. 

```
class Solution {
public:
    int climbStairs(int n) {
        std::unordered_map<int,int> hash = 
        {
	        {1, 1},
	        {2, 2}
        };
        
        if(n == 1 || n == 2){
	        return hash.find(n)->second;
        }
	
		for(int i = 3; i <= n; i++){
			hash.insert({i,hash.find(i-1)->second + hash.find(i-2)->second});
		}
		
		return hash.find(n-1)->second + hash.find(n-2)->second;
        
    }
};
```

This solution works, but there's actually zero reason to use a hash map other than to overcomplicate things. We can just use an array:
```
class Solution {
public:
    int climbStairs(int n) {
        std::vector<int> arr(n);

        if(n == 1 || n == 2){
	        return n;
        }

        arr[0] = 1; 
        arr[1] = 2;
	
		for(int i = 2; i < n; i++){
			arr[i] = arr[i-1] + arr[i-2];
		}
		
		return arr[n-1];
        
    }
};
```

We needed to make sure that we returned n in the cases n is 1 or 2 BEFORE we declare the values for arr[0] or arr[1] because that could cause undetermined behavior (which is bad). 

This solution just iterates through n, so the complexity is O(n). 

We can actually implement a solution that doesn't need the for loop checking for n = 1 or n=2 having f(0) = 1 and extending size of the array by one. 

```
class Solution {
public:
    int climbStairs(int n) {
        std::vector<int> arr(n+1);
        arr[0] = arr[1] = 1;
	
		for(int i = 2; i <= n; i++){
			arr[i] = arr[i-1] + arr[i-2];
		}
		
		return arr[n];
        
    }
};
```