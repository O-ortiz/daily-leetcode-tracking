Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 3

**Example 2:**

**Input:** root = [1,null,2]
**Output:** 2

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`


---

This is a problem that can be solved recursively as well. When given a node you can traverse through to the next node left or right if they exist and add one every node until you reach a leaf node. You take the max of the left or right and add it to your count. 


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
    int maxDepth(TreeNode* root) {
	    int l=0;
	    int r=0;
	    
		if(!root){
			return 0;
		}
	    
	    if(!root->left && !root->right){
		    return 1;
	    }
	    
	    if(root->left){
		    l = maxDepth(root->left);
	    }
	    
	    if(root->right){
		    r = maxDepth(root->right);
	    }
	    
	    
	    return 1 + std::max(l,r);
	    
        
    }
};
```

Root might be a nullptr to begin with so we need the edge case for that. Then we check if it is a leaf node. For some reason, our answer if one less than expected. Are we not adding 1 when we should? OMG we did it again,  r = maxDepth(root->left) should be  r = maxDepth(root->right). 

Dang it why don't you pay more attention. 

We could simplify the code a bit now by just just calling maxDepth even on a nullptr since it already check if !root. 

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
	    int l=0;
	    int r=0;
	    
		if(!root){
			return 0;
		}
	    
	    if(!root->left && !root->right){
		    return 1;
	    }
	    
		l = maxDepth(root->left);
		r = maxDepth(root->right);
	    
	    return 1 + std::max(l,r);
	    
    }
};
```

This is O(n) since it just traverses each node, where n is number of nodes. 

We can simplify this further for no real reason by just returning std::max(maxDepth(root->left), maxDepth(root->right)).

