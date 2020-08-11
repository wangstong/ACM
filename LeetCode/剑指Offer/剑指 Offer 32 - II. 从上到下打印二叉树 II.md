### 剑指 Offer 32 - II. 从上到下打印二叉树 II
从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
例如:
给定二叉树:` [3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层次遍历结果：
```
[
  [3],
  [9,20],
  [15,7]
]
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (root == nullptr) return res;

        queue<TreeNode*> que; que.push(root);
        while (!que.empty()) {
            vector<int> tmp;
            int len = que.size();
            for (int i = 1; i <= len; ++i) {
                TreeNode* ptr = que.front(); que.pop();
                if (ptr->left != nullptr) que.push(ptr->left);
                if (ptr->right != nullptr) que.push(ptr->right);
                tmp.push_back(ptr->val);
            }
            res.push_back(tmp);
        }

        return res;
    }
};
```

