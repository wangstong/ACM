### 剑指 Offer 32 - III. 从上到下打印二叉树 III
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
例如:
给定二叉树: `[3,9,20,null,null,15,7]`,
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
  [20,9],
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
        int height = 0;
        while (!que.empty()) {
            vector<int> tmp;
            int len = que.size();
            for (int i = 1; i <= len; ++i) {
                TreeNode* ptr = que.front(); que.pop();
                if (ptr->left != nullptr) que.push(ptr->left);
                if (ptr->right != nullptr) que.push(ptr->right);
                tmp.push_back(ptr->val);
            }
            if (height % 2 == 1) reverse(tmp.begin(),tmp.end());
            res.push_back(tmp);
            height++;
        }

        return res;
    }
};
```