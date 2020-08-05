### 剑指 Offer 34. 二叉树中和为某一值的路径
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。
**示例:**
给定如下二叉树，以及目标和 sum = 22，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
返回:
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
**提示：**
* `节点总数 <= 10000`

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
    vector<vector<int>> res;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> tmp;
        dfs(root,sum,tmp);
        return res;
    }
    void dfs(TreeNode* root, int sum, vector<int>& tmp) {
        if (root == nullptr) return;
        if (sum-root->val == 0 && root->left == nullptr && root->right == nullptr) {
            tmp.push_back(root->val);
            res.push_back(tmp);
            tmp.pop_back();
            return;
        }

        tmp.push_back(root->val);
        dfs(root->left, sum-root->val, tmp);
        dfs(root->right, sum-root->val, tmp);
        tmp.pop_back();
    }
};
```