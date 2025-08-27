You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

**Input:** list1 = [1,2,4], list2 = [1,3,4]
**Output:** [1,1,2,3,4,4]

**Example 2:**

**Input:** list1 = [], list2 = []
**Output:** []

**Example 3:**

**Input:** list1 = [], list2 = [0]
**Output:** [0]

---

The lists are given to us as singly linked lists. This means we need to traverse through the lists node by node and change the pointers for the next node to 'zip' the lists together. 

We need two pointers to track where we are in each list. Ptr1 keeps track of the current place in list1, and ptr2 keeps track of our current place in the given lists. 

We make a new ListNode * object that will be the head of our list. As we go down the two lists, we take the listnode object with the smalest value and set that as the next pointer for the list. We then move the ptr for the list we took the list node from to the next node. We continue until we reach the end of both lists. 

It helps us to have a temporary list node object. 


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
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* prt1 = list1;
        ListNode* prt2 = list2;
        ListNode* head = new ListNode;
        
        while(ptr1 || ptr2){
	        if (pt1 && ptr2){
		        if (pt1->val < pt2->val){
			         if(!head->next){
				         head=pt1;
				         pt1 = pt1->next;
			         }
			         else{
				         pt1->next = 
			         }
			        
		        }
		        else{
			        
		        }
		        
	        }
	        else if(pt1 && !ptr1){
	        
	        }
	        else({
	        
	        }
        }
        
        return head; 
    }
};
```

