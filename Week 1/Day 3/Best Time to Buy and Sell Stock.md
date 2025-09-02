Given: an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
Return: _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 5
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transactions are done and the max profit = 0.

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

---

We could take the naive approach and brute force this, which could likely be O(n^2). Instead, we need to think more deeply about our approach. 

Since we can chose to not sell (output = 0), we have that be the default for our return. 

We could try keeping track of all of the differences between a number and every other number after it in the array, but we only really care about the profit made further down the array.  

To do this, we only need to keep track of the largest profit we've found so far as we traverse the array. Since we don't need to keep track of indices, this simplifies the solution to use only a max() function between the current max value (ans) and the difference between the int we are currently looking at in the array and the smallest value seen before this index. If this difference is larger (ie this is now the largest sell value we've found), then we take this new difference as the max value and set it to ans. 


```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
	    int ans = 0;
	    int lowest = prices[0];
	    
	    for(int i=0; i < prices.size(); i++){
		    if( prices[i] < lowest){
			    lowest = prices[i];
		    }
		    ans = std::max(ans, prices[i]-lowest);
	    }
	    
	    return ans;
        
    }
};
```

This answer is O(n) since we traverse the array once. 