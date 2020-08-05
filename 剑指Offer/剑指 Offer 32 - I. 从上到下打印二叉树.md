### 剑指 Offer 32 - I. 从上到下打印二叉树
从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。
例如:
给定二叉树: `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回：
```
[3,9,20,15,7]
```
**提示：**
* `节点总数 <= 1000`
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
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) return res;

        queue<TreeNode*> que; que.push(root);
        while (!que.empty()) {
            int len = que.size();
            for (int i = 1; i <= len; ++i) {
                TreeNode* ptr = que.front(); que.pop();
                if (ptr->left != nullptr) que.push(ptr->left);
                if (ptr->right != nullptr) que.push(ptr->right);
                res.push_back(ptr->val);
            }
        }

        return res;
    }
};
```