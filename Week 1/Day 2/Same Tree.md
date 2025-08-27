Given: roots of two binary trees `p` and `q`
Return: Bool of if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

**Input:** p = [1,2,3], q = [1,2,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

**Input:** p = [1,2], q = [1,null,2]
**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

**Input:** p = [1,2,1], q = [1,1,2]
**Output:** false

**Constraints:**

- The number of nodes in both trees is in the range `[0, 100]`.
- `-104 <= Node.val <= 104`
---

To solve this problem we have to traverse through the nodes of both at the same time. Every time we detect that either the value or the structure are not the same, we return false. If we reach the end of both trees, we return true. As long as we traverse and check the entire tree, the order (Breath First, or Depth First) does not matter. 

Let's do Breath First, meaning we use queue of TreeNode* objects. 

First we also need to check edge cases where p and q are both empty. Technically, they are equal. If p or q are empty, but not both, then immediately return false. (Funny enough, this alone plus a default false return passes 54 cases of 67.) 

Then we traverse through the tree by using a while loop while either queue is non empty. Inside we then check if only one but not the other is a nullptr. This means that the structure is not the same, and we immediately return false. 

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
	    std::queue<TreeNode*> que1;
	    std::queue<TreeNode*> que2;
		
		if(!p && !q){
			return true;
		}
		if(!p || !q){
			return false;
		}
  
	    que1.push(p);
	    que2.push(q);
	    
		while (!que1.empty() || !que2.empty()){
			if(que1.empty() || que2.empty()){
				return false;
			}
			else{
				
				if(que1.front()->val != que2.front()->val){
					return false;
				}
				
				if(que1.front()->left && que2.front()->left){
					que1.push(que1.front()->left);
					que2.push(que2.front()->left);	
				}
				
				if(que1.front()->right && que2.front()->right){
					que1.push(que1.front()->left);
					que2.push(que2.front()->left);	
				}
				
				que1.pop();
				que2.pop();
			}
		}
		return true;
        
    }
};
```

This solutions has issues with inputs where one of the values is "null". For example, when p =[1,2] and q = [1, null, 2]. Let's see if we can debug why. 

Tree p has root node with val 1 and a left node with val 2. 
Tree 1 has a root node with value 1, a left node with value null, and a right node with value 2. 

We reach the while loop with no problems, with que1 containing root node p with val 1. que2 contains root node q with val 1. The problem here is we are not catching when one has a left or right node that the other doesn't. 


```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
	    std::queue<TreeNode*> que1;
	    std::queue<TreeNode*> que2;
		
		if(!p && !q){
			return true;
		}
		if(!p || !q){
			return false;
		}
  
	    que1.push(p);
	    que2.push(q);
	    
		while (!que1.empty() || !que2.empty()){
			if(que1.empty() || que2.empty()){
				return false;
			}
			else{
				
				if(que1.front()->val != que2.front()->val){
					return false;
				}
				
				if((que1.front()->left && !que2.front()->left) || (que2.front()->left && !que1.front()->left)){
					return false;
				}
				
				if((que1.front()->right && !que2.front()->right) || (que2.front()->right && !que1.front()->right)){
					return false;
				}
			
				if(que1.front()->left && que2.front()->left){
					que1.push(que1.front()->left);
					que2.push(que2.front()->left);	
				}
				
				if(que1.front()->right && que2.front()->right){
					que1.push(que1.front()->right);
					que2.push(que2.front()->right);	
				}
				
				que1.pop();
				que2.pop();
			}
		}
		return true;
        
    }
};
```

For some reason this causes a null pointer access issue for p = [1,null,1], q = [1,null,1], particularly this line: if(que1.front()->val != que2.front()->val). 

We are trying to access the value of a nullptr at some point. This is undefined behavior and really dangerous. The problem here is likely due to both having null left side nodes. 

OH

turns out the problem was literally just a typo. I accidentally had que1.push(que1.front()->left), when I meant que1.push(que1.front()->right). 

Anyways this solution works, though I wonder if we can optimize it by not using a queue to Breath First traverse through the two trees, but instead recursively calling isSameTree and "anding" the returns. 

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
    
		if(!p && !q){
			return true;
		}
		if(!p || !q){
			return false;
		}
		
		if(p->val != q->val){
			return false;
		}

		bool1 = isSameTree(p->left, q->left);
		bool2 = isSameTree(p->right, q->right);
		
		return bool1 && bool2;
		
};

```

This works and is runtime O(n) where n is the number of nodes in one tree. 