### 剑指 Offer 54. 二叉搜索树的第k大节点
给定一棵二叉搜索树，请找出其中第k大的节点。

**示例 1:**
```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```
**示例 2:**
```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

**限制：**

* `1 ≤ k ≤ 二叉搜索树元素个数`

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int count = 0, ans = 0;
    int kthLargest(TreeNode* root, int k) {
        dfs(root,k);
        return ans;
    }
    bool dfs(TreeNode* root, int k) {
        if (root == nullptr) return false;
        
        if (dfs(root->right,k) == true) return true;
        if (++count == k) {
            ans = root->val;
            return true;
        }
        if (dfs(root->left,k) == true) return true;
        
        return false;
    }
};
```