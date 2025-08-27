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
        ListNode* curr = head;       
        while(ptr1 || ptr2){
	        if (pt1 && ptr2){
		        if (pt1->val < pt2->val){
			         if(!head->next){
				         head=pt1;
				         pt1 = pt1->next;
			         }
			         else{
				         temp = pt1->next;
				         pt1->next = pt1;
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

We started with that solution but I quickly realized that we can remove pt1 and pt2 and directly use list1 and list2. Also, we can directly skip the step to check if the head is the first empty node, and just directly start setting the next node starting from head (which should initially be nullptr) and at the end just return head->next. 

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
        ListNode* head = new ListNode;
        ListNode* curr = head;
        
	    
		while(list1 && list2){
			if(list1->val < list2->val){
				curr->next = list1;
				list1 = list1->next;
			}
			else{
				curr->next = list2;
				list2 = list2->next;
			}
			curr=curr->next;
		}
		
		while(list1 || list2){
			if(list1){
				curr->next=list1;
				list1= list1->next;
			}
			else if(list2){
				curr->next =list2;
				list2 = list2->next;
			}
		}
		
		return head->next;
 
    }
};
```

This uses two while loops, but could be improved with just one while loop like our original attempt. Also, somehow this solution misses a lot of hidden test cases, so let's debug why it might fail something like: list1 = [-9,3], list2 =[5,7]. Looking through the code it's obvious why: since we have two while loops if one of the lists is completely iterated over before the other, we move on to the next while loop. This means that curr is set to next which is nullptr. We THEN inside the if loops set curr->next, which is taking the next value of a nullptr. Which is undefined behavior. 

So let's make a solution with just one for loop and check if both lists are not nullptr. 

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
        ListNode* head = new ListNode;
        ListNode* curr = head;
		
		while(list1 || list2){
		
			if(list1 && list2){
				if(list1->val < list2->val){
					curr->next = list1;
					list1 = list1->next;
				}
				else{
					curr->next = list2;
					list2 = list2->next;
				}
			}
			else if(list1 && !list2){
				curr->next=list1;
				list1= list1->next;
			}
			else{
				curr->next =list2;
				list2 = list2->next;
			}
			
			curr=curr->next;
		}
		
		return head->next;
 
    }
};

```

This solution now merges them into one single while loop, and now only has one call of head->next at the end of the loop without introducing undefined behavior like we saw in the two while loop solution. This solution is in O(n) since we traverse each list only once where n is the total number of nodes. 

