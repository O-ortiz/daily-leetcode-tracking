Given: `head`, the head of a linked list, determine if the linked list has a cycle in it.
Return: `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.



**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** false
**Explanation:** There is no cycle in the linked list.

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?


---

This problem has the tags "hash table", "linked list", and "two pointers" do I finally get to use a hash table again?? Hooray. 

Okay so we need to traverse a linked list and determine if there is a loop. We can build a hash of every node we've traversed so far. The key can be a ListNode* type and the value doesn't actually matter. For every node, we look at the address of the next ptr and it it's one we have seen before we return false, and if it's not we move on to the next and add the current node to the list. We keep going until we reach the end of the list if it has one. 

The important part is that we can search for the key in the hash in O(1) constant time. 

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        std::unordered_map<ListNode*, int> hash;
        if(!head || !head->next){ return false;}
        while(head){

	        hash.insert({head, head->val});
            if(hash.find(head->next) != hash.end()){
		        return true;
	        }
	        head = head->next;
        }
        
        return false;
        
    }
};
```

Yeah this solution works and I did finally get to use a hash map! Except, there's actually a much better one that does use a two pointer solution and with constant memory too. My shame at not knowing this is immeasurable. 

This is the hare and tortoise problem, and the reason this is ALSO tagged as a two pointer problem. Basically, we have a tortoise pointer that traverses the list normally, one node at a time. We also have a hare pointer that traverses the list TWO nodes at a time. The thinking here is that if there IS a loop, then the hare will eventually "catch up" to the tortoise. If there is no loop, then we  detect that the hare cannot move two nodes and return true because finding the end of the list effectively shows that there is no loop. 

This is pretty smart and I'd have never though of it, though I should have picked up on it with that "two pointers" tag but I'm not THAT smart. 

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {		

        ListNode* tort = head;
		ListNode* hare = head;
		
		while(hare && hare->next){

			tort=tort->next;
            hare = hare->next->next;

            if(hare == tort){
				return true;
			}
		}
        return false;
    }
};
```

