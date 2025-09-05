Given the `root` of a binary tree, invert the tree, and return _its root_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

**Input:** root = [4,2,7,1,3,6,9]
**Output:** [4,7,2,9,6,3,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

**Input:** root = [2,1,3]
**Output:** [2,3,1]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

---

We can solve this one using recursion. Wait... can we? 

Thinking this through we can see that it should be as simple as swapping the pointers for the left and right nodes of a root node. We can't directly call invertTree and set the root->left and root->right values because we need to swap them. 

So we first make two temporary pointers that call the recursion of the left and right nodes, then set those to left and right but swapped. 

We also need to handle the base case, which is when root is nullptr, to just immediately return.

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
    TreeNode* invertTree(TreeNode* root) {
        if(!root){
            return root;
        }
        TreeNode* temp_left = invertTree(root->right);
        TreeNode* temp_right = invertTree(root->left);
        root->left = temp_left;
        root->right = temp_right;

        return root;
    }
};
```

This works and is O(n). We could have also used std::swap and directly called invertTree if we called it for the same pointer (left/right). 


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
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return root;
        
        root->left = invertTree(root->left);
        root->right = invertTree(root->right);
        
        swap(root->left,root->right);
        return root;
    }
};
```



