Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?


---
Ooooh, reverse linked list my beloved we meet again. 

First, let's tackle the iterative solution before the recursive one. 

We can simply keep two pointers, one for the current node, and one for the previous one. Each time we loop, we set the next node after the previous to a temporary node. Set the value of current->next to previous. And set current to the node in temporary. 

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
	    ListNode* curr = head;
	    ListNode* prev = nullptr;
	    while(curr){
			ListNode* temp = curr->next;
			curr->next = prev;
			curr= temp;	
	    }
	    
	    return curr;
        
    }
};
```

Make sure that prev is initially nullptr so that it doesn't resort to a default constructor and make it point to a ListNode* object. We could solve this recursively but this is a better solution. 