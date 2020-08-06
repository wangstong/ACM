### 剑指 Offer 37. 序列化二叉树
请实现两个函数，分别用来序列化和反序列化二叉树。
示例:
```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        queue<TreeNode*> que;
        if (root != nullptr) que.push(root);

        while (!que.empty()) {
            TreeNode * x = que.front();
            que.pop();

            if (x != nullptr) {
                res += to_string(x->val) + ',';
                que.push(x->left);
                que.push(x->right);
            }
            else {
                res += "null,";
            }
        }

        if (!res.empty()) res.pop_back();
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        TreeNode * ptr = new TreeNode(0);
        queue<TreeNode*> que; que.push(ptr);
        int start = 0, end = 0;
        bool flag = false;
        while (start < data.size()) {
            while (end < data.size() && data[end] != ',') end++;
            string str = data.substr(start, end-start);
            
            TreeNode * ptr1 = nullptr;         
            if (str != "null") ptr1 = new TreeNode(atoi(str.c_str()));
            
            TreeNode * ptr2 = que.front();
            if (flag) {
                ptr2->left = ptr1;
            }
            else {
                ptr2->right = ptr1;
                que.pop();
            }

            if (ptr1 != nullptr) que.push(ptr1);
            flag = !flag;
            start = ++end;
        }
        return ptr->right;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

