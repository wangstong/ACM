[TOC]
### 剑指 Offer 03. 数组中重复的数字

找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**
```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

**限制：**
```
2 <= n <= 100000
```

**代码：**
```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int len = nums.size(), res;
        for (int i = 0; i < len; ++i) {
            if (nums[i] == nums[nums[i]] && i != nums[i]) {
                res = nums[i];
                break;
            }
            else swap(nums[i],nums[nums[i]]);
        }
        return res;
    }
};
```

### 剑指 Offer 04. 二维数组中的查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**示例:**

现有矩阵 matrix 如下：
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。


**限制：**

`0 <= n <= 1000`
`0 <= m <= 1000`

```cpp
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0) return false;
        int m = matrix.size(), n = matrix[0].size();
        for (int i = 0; i < m; ++i) {
            int pos = lower_bound(matrix[i].begin(),matrix[i].end(),target) - matrix[i].begin();
            if (pos == n) continue;
            if (matrix[i][pos] == target) return true; 
        }
        return false;
    }
};
```

### 剑指 Offer 05. 替换空格
请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

**示例 1：**
```
输入：s = "We are happy."
输出："We%20are%20happy."
```

**限制：**

`0 <= s 的长度 <= 10000`

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == ' ') res += "%20";
            else res += s[i];
        }
        return res;
    }
};
```

### 剑指 Offer 06. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

**示例 1：**
```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

`0 <= 链表长度 <= 10000`

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int> res;
        while (head != nullptr) {
            res.insert(res.begin(),head->val);
            head = head->next;
        }
        return res;
    }
};
```

### 剑指 Offer 07. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出
```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```
返回如下的二叉树：
```
    3
   / \
  9  20
    /  \
   15   7
```

**限制：**

`0 <= 节点个数 <= 5000`

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return build(preorder, inorder, 0, 0, inorder.size()-1);
    }
    TreeNode* build(vector<int>& preorder, vector<int>& inorder, int root, int st, int ed)
    {
        if (st > ed) return nullptr;
        TreeNode * tree = new TreeNode(preorder[root]);
        int pos = st;
        while (pos < ed && preorder[root] != inorder[pos]) ++pos;
        tree->left = build(preorder, inorder, root+1, st, pos-1);
        tree->right = build(preorder, inorder, root+1+pos-st, pos+1, ed);
        return tree;
    }
};
```

### 剑指 Offer 09. 用两个栈实现队列
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

**示例 1：**
```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

**示例 2：**
```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

**提示：**
* 1 <= values <= 10000
* 最多会对 appendTail、deleteHead 进行 10000 次调用

```cpp
class CQueue {
public:
    stack<int> st1, st2;
    CQueue() {
    }
    
    void appendTail(int value) {
        st1.push(value);
    }
    
    int deleteHead() {
        if (st1.empty() && st2.empty()) return -1;
        if (st2.empty()) {
            while (!st1.empty()) {
                st2.push(st1.top());
                st1.pop();
            }
        }
        int res = st2.top();
        st2.pop();
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

### 剑指 Offer 10- I. 斐波那契数列
请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。


**示例 1：**
```
输入：n = 2
输出：1
```

**示例 2：**
```
输入：n = 5
输出：5
```

**提示：**

`0 <= n <= 100`

```cpp
class Solution {
public:
    int mod = 1e9+7;
    int fib(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        vector<int> dp(n+1);
        dp[0] = 0, dp[1] = 1;
        for (int i = 2; i <= n; ++i) dp[i] = (dp[i-1]+dp[i-2])%mod;
        return dp[n];
    }
};
```

### 剑指 Offer 10- II. 青蛙跳台阶问题
一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

**示例 1：**
```
输入：n = 2
输出：2
```

**示例 2：**
```
输入：n = 7
输出：21
```
**提示：**

`0 <= n <= 100`

```cpp
class Solution {
public:
    int numWays(int n) {
        if (n == 0 || n == 1) return 1;
        vector<int> dp(n+1);
        dp[1] = 1, dp[2] = 2;
        for (int i = 3; i <= n; ++i) {
            dp[i] = (dp[i-1] + dp[i-2]) % 1000000007;
        }
        return dp[n];
    }
};
```

### 剑指 Offer 11. 旋转数组的最小数字
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组`[3,4,5,1,2]`为`[1,2,3,4,5]`的一个旋转，该数组的最小值为1。  

**示例 1：**
```
输入：[3,4,5,1,2]
输出：1
```
**示例 2：**

```
输入：[2,2,2,0,1]
输出：0
```

```cpp
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int l = 0, r = numbers.size()-1;
        while (l < r) {
            int mid = l+r>>1;
            if (numbers[mid] > numbers[r]) {
                l = mid+1;
            }
            else if (numbers[mid] < numbers[r]) {
                r = mid;
            }
            else --r;
        }
        return numbers[l];
    }
};
```

### 剑指 Offer 12. 矩阵中的路径
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

`[["a","b","c","e"],`
`["s","f","c","s"],`
`["a","d","e","e"]]`

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

**示例 1：**
```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```
**示例 2：**
```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```
**提示：**

`1 <= board.length <= 200`
`1 <= board[i].length <= 200`

```cpp
class Solution {
public:
    bool vis[200][200], flag;
    int m, n;
    bool exist(vector<vector<char>>& board, string word) {
        memset(vis,0,sizeof(vis));
        m = board.size(), n = board[0].size();
        flag = false;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                dfs(board,i,j,word,0);
            }
        }
        return flag;
    }
    void dfs(vector<vector<char>>& board, int x, int y, string& word, int pos) {
        if (flag) {
            return;
        }
        
        if (x < 0 || x >= m || y < 0 || y >= n || vis[x][y] || board[x][y] != word[pos]) {
            return;
        }

        if (pos == word.size()-1) {
            flag = true;
            return;
        }

        vis[x][y] = true;
        dfs(board,x+1,y,word,pos+1);
        dfs(board,x-1,y,word,pos+1);
        dfs(board,x,y+1,word,pos+1);
        dfs(board,x,y-1,word,pos+1);
        vis[x][y] = false;
    }
};
```

### 剑指 Offer 13. 机器人的运动范围
地上有一个m行n列的方格，从坐标 `[0,0] `到坐标 `[m-1,n-1] `。一个机器人从坐标` [0, 0] `的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

**示例 1：**
```
输入：m = 2, n = 3, k = 1
输出：3
```
**示例 2：**
```
输入：m = 3, n = 1, k = 0
输出：1
```
**提示：**

* `1 <= n,m <= 100`
* `0 <= k <= 20`

```cpp
class Solution {
public:
    bool vis[100][100];
    int movingCount(int m, int n, int k) {
        memset(vis,0,sizeof(vis));
        return dfs(0, 0, m, n, k);
    }
    int dfs(int x, int y, int m, int n, int k) {
        if (x < 0 || y < 0 || x >= m || y >= n || (x/10 + x%10 + y/10 + y%10) > k || vis[x][y] == true) return 0;
        vis[x][y] = true;
        return 1 + dfs(x+1,y,m,n,k) + dfs(x-1,y,m,n,k) + dfs(x,y+1,m,n,k) + dfs(x,y-1,m,n,k);
    }
};
```

### 剑指 Offer 14- I. 剪绳子
给你一根长度为` n `的绳子，请把绳子剪成整数长度的` m `段（m、n都是整数，n>1并且m>1），每段绳子的长度记为` k[0],k[1]...k[m-1] `。请问` k[0]*k[1]*...*k[m-1] `可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

**示例 1：**
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```

**示例 2:**
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

**提示：**
* `2 <= n <= 58`

```cpp
class Solution {
public:
    int cuttingRope(int n) {
        if (n == 2) return 1;
        else if (n == 3) return 2;

        if (n % 3 == 0) return pow(3,n/3);
        else if (n % 3 == 1) return pow(3,n/3-1) * 4;
        else return pow(3,n/3) * 2;
    }
};
```

### 剑指 Offer 14- II. 剪绳子 II
给你一根长度为` n `的绳子，请把绳子剪成整数长度的` m `段（m、n都是整数，n>1并且m>1），每段绳子的长度记为` k[0],k[1]...k[m - 1] `。请问` k[0]*k[1]*...*k[m - 1] `可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

**示例 1：**
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```
**示例 2:**
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

**提示：**

* `2 <= n <= 1000`

```cpp
class Solution {
public:
    long long qmul(long long a, long long b, long long p = 1000000007) {
        long long res = 0;
        while (b) {
            if (b & 1) res = (res+a)%p;
            a = (a+a)%p;
            b >>= 1;
        }
        return res%p;
    }
    long long qpow(long long a, long long b, long long p = 1000000007) {
        long long res = 1;
        while (b) {
            if (b & 1) res = res*a%p;
            a = a*a%p;
            b >>= 1;
        }
        return res%p;
    }
    int cuttingRope(int n) {
        if (n == 2) return 1;
        else if (n == 3) return 2;

        if (n % 3 == 0) return qpow(3,n/3);
        else if (n % 3 == 1) return qmul(qpow(3,n/3-1),4);
        else return qmul(qpow(3,n/3),2);
    }
};
```

### 剑指 Offer 15. 二进制中1的个数
请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

**示例 1：**
```
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```
**示例 2：**
```
输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```
**示例 3：**
```
输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
```

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while (n) {
            if (n & 1) ++res;
            n >>= 1;
        }
        return res;
    }
};
```

### 剑指 Offer 16. 数值的整数次方
实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

 

**示例 1:**
```
输入: 2.00000, 10
输出: 1024.00000
```
**示例 2:**
```
输入: 2.10000, 3
输出: 9.26100
```
**示例 3:**
```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```

**说明:**

* `-100.0 < x < 100.0`
* `n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。`


```cpp
class Solution {
public:
    double qmul1(double a, int b) {
        double res = 1.0;
        while (b) {
            if (b & 1) res = res*a;
            a = a*a;
            b >>= 1;
        }
        return res;
    }
    double qmul2(double a, long b) {
        double res = 1.0;
        b = -b;
        while (b) {
            if (b & 1) res = res/a;
            a = a*a;
            b >>= 1;
        }
        return res;
    }
    double myPow(double x, int n) {
        return n >= 0 ? qmul1(x,n) : qmul2(x,n);
    }
};
```

### 剑指 Offer 17. 打印从1到最大的n位数
输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

**示例 1:**
```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

**说明：**

* 用返回一个整数列表来代替打印
* n 为正整数

```cpp
class Solution {
public:
    vector<int> printNumbers(int n) {
        int lim = pow(10,n);
        vector<int> res;
        for (int i = 1; i < lim; ++i) res.push_back(i);
        return res;
    }
};
```

### 剑指 Offer 18. 删除链表的节点
给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

注意：此题对比原题有改动

**示例 1:**
```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

**示例 2:**
```
输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

**说明：**

* 题目保证链表中节点的值互不相同
* 若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        if (head->val == val) return head->next;
        ListNode * ptr = head;
        while (ptr->next->val != val) ptr = ptr->next;
        ptr->next = ptr->next->next;
        return head;
    }
};
```

### 剑指 Offer 19. 正则表达式匹配
请实现一个函数用来匹配包含`'. '`和'`*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。

**示例 1:**
```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
**示例 2:**
```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```
**示例 3:**
```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```
**示例 4:**
```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```
**示例 5:**
```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```
* s 可能为空，且只包含从 a-z 的小写字母。
* p 可能为空，且只包含从 a-z 的小写字母以及字符 . 和 *，无连续的 '*'。

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        return isMatchHelper(s, 0, p, 0);
    }
    bool isMatchHelper(string & s, int ps, string & p, int pp) {
        // 完全匹配
        if (ps == s.size() && pp == p.size()) {
            return true;
        }
        // 不完全匹配
        if (ps != s.size() && pp == p.size()) {
            return false;
        }
        // 当前字符匹配判断
        bool flag = ps < s.size() && (s[ps] == p[pp] || p[pp] == '.');
        if (p.size() - pp >= 2 && p[pp+1] == '*') {
            // 匹配0次或多次
            return isMatchHelper(s, ps, p, pp+2) || (flag && isMatchHelper(s, ps+1, p, pp));
        }
        return flag && isMatchHelper(s, ps+1, p, pp+1);
    }
};
```

### 剑指 Offer 20. 表示数值的字符串
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"、"-1E-16"及"12e+5.4"都不是。

```py
class Solution:
    def isNumber(self, s: str) -> bool:
        try:
            float(s)
            return True
        except:
            return False
```

### 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**示例：**
```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

**提示：**
* `1 <= nums.length <= 50000`
* `1 <= nums[i] <= 10000`


```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int len = nums.size();
        int l = 0, r = len-1;
        while (l < r) {
            while (nums[l] % 2 == 1 && l < r) ++l;
            while (nums[r] % 2 == 0 && l < r) --r;
            if (l < r) swap(nums[l],nums[r]);
        }
        return nums;
    }
};
```

### 剑指 Offer 22. 链表中倒数第k个节点
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

**示例：**
```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode * ptr1 = head, * ptr2 = head;
        while (ptr1 != nullptr && k > 0) {
            ptr1 = ptr1->next;
            --k;
        }
        while (ptr1 != nullptr) {
            ptr1 = ptr1->next;
            ptr2 = ptr2->next;
        }
        return ptr2;
    }
};
```

### 剑指 Offer 24. 反转链表
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

**示例:**
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**限制：**

* `0 <= 节点个数 <= 5000`

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode * pre = head;
        if (head == nullptr || head->next == nullptr) return head;
        ListNode * cur = head->next, * next = head->next->next;

        head->next = nullptr;
        while (233) {
            cur->next = pre;
            
            if (next == nullptr) break;
            pre = cur;
            cur = next;
            next = next->next;
        }
        return cur;
    }
};
```

### 剑指 Offer 25. 合并两个排序的链表
输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

**示例1：**
```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```
**限制：**

`0 <= 链表长度 <= 1000`

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == nullptr) return l2;
        else if (l2 == nullptr) return l1;
        
        ListNode * ptr, * res;
        if (l1->val > l2-> val) ptr = l2, l2 = l2->next;
        else ptr = l1, l1 = l1->next;
        res = ptr;
        while (l1 && l2) {
            if (l1->val > l2-> val) ptr->next = l2, l2 = l2->next;
            else ptr->next = l1, l1 = l1->next;
            ptr = ptr->next;
        }
        while (l1) {
            ptr->next = l1, l1 = l1->next;
            ptr = ptr->next;
        }
        while (l2) {
            ptr->next = l2, l2 = l2->next;
            ptr = ptr->next;
        }
        return res;
    }
};
```

### 剑指 Offer 26. 树的子结构
输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:
```
     3
    / \
   4   5
  / \
 1   2
```
给定的树 B：
```
   4 
  /
 1
```
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

**示例 1：**
```
输入：A = [1,2,3], B = [3,1]
输出：false
```
**示例 2：**
```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```
**限制：**

`0 <= 节点个数 <= 10000`

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
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (A == nullptr || B == nullptr) return false;
        return dfs(A,B) || isSubStructure(A->left,B) || isSubStructure(A->right,B);
    }
    bool dfs(TreeNode* A, TreeNode* B) {
        if (B == nullptr) return true;
        if (A == nullptr) return false;
        return A->val == B->val && dfs(A->left,B->left) && dfs(A->right,B->right);
    }
};
```

### 剑指 Offer 27. 二叉树的镜像
请完成一个函数，输入一个二叉树，该函数输出它的镜像。
例如输入：
```

     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
镜像输出：
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
**示例 1：**
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```
**限制：**
`0 <= 节点个数 <= 1000`

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
    TreeNode* mirrorTree(TreeNode* root) {
        if (root == nullptr) return root;
        swap(root->left,root->right);
        mirrorTree(root->left);
        mirrorTree(root->right);
        return root;
    }
};
```

### 剑指 Offer 28. 对称的二叉树
请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```

    1
   / \
  2   2
   \   \
   3    3
```

**示例 1：**
```
输入：root = [1,2,2,3,4,4,3] 
输出：true
```
**示例 2：**
```
输入：root = [1,2,2,null,3,null,3] 
输出：false
```
**限制：**
`0 <= 节点个数 <= 1000`

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
    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) return true;
        return dfs(root->left,root->right);
    }
    bool dfs(TreeNode* root1, TreeNode* root2) {
        if (root1 == nullptr && root2 == nullptr) return true;
        if (root1 == nullptr || root2 == nullptr) return false;
        if (root1->val == root2->val && dfs(root1->left,root2->right) && dfs(root1->right,root2->left)) return true;
        return false;
    }
};
```

### 剑指 Offer 29. 顺时针打印矩阵
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
**示例 1：**
```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```
**示例 2：**
```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```
**限制：**
* `0 <= matrix.length <= 100`
* `0 <= matrix[i].length <= 100`

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector <int> res;
        if (matrix.empty()) return res;
        int rl = 0, rh = matrix.size()-1; 
        int cl = 0, ch = matrix[0].size()-1;
        while (1) {
            for (int i = cl; i <= ch; ++i) res.push_back(matrix[rl][i]);
            if (++rl > rh) break; 
            for (int i = rl; i <= rh; ++i) res.push_back(matrix[i][ch]);
            if (--ch < cl) break;
            for (int i = ch; i >= cl; --i) res.push_back(matrix[rh][i]);
            if (--rh < rl) break;
            for (int i = rh; i >= rl; --i) res.push_back(matrix[i][cl]);
            if (++cl > ch) break;
        }
        return res;
    }
};
```

### 剑指 Offer 30. 包含min函数的栈
定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
**示例:**
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min(); --> 返回 -3. 
minStack.pop(); 
minStack.top(); --> 返回 0. 
minStack.min(); --> 返回 -2.
```
**提示：**
* `各函数的调用总次数不超过 20000 次`

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> st, min_st;
    MinStack() {

    }
    
    void push(int x) {
        if (min_st.empty() || x <= min_st.top()) min_st.push(x);
        st.push(x);
    }
    
    void pop() {
        if (st.top() == min_st.top()) min_st.pop();
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int min() {
        return min_st.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

### 剑指 Offer 31. 栈的压入、弹出序列
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。
**示例 1：**
```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true 
解释：我们可以按以下顺序执行： 
push(1), push(2), push(3), push(4), pop() -> 4, 
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```
**示例 2：**
```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false 解释：1 不能在 2 之前弹出。
```
**提示：**
* `0 <= pushed.length == popped.length <= 1000`
* `0 <= pushed[i], popped[i] < 1000`
* `pushed 是 popped 的排列。`

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st;
        int i = 0, j = 0;
        while (233) {
            if (i < pushed.size() && (st.empty() || st.top() != popped[j])) {
                st.push(pushed[i]);
                i++;
            }

            if (j < popped.size() && st.top() == popped[j]) {
                st.pop();
                j++;
            }
            if (i == pushed.size() && j == popped.size()) {
                return true;
            }
            if (i == pushed.size() && j < popped.size() && st.top() != popped[j]) {
                break;
            }
        }
        return false;
    }
};
```

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

### 剑指 Offer 33. 二叉搜索树的后序遍历序列
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。
参考以下这颗二叉搜索树：
```
     5
    / \
   2   6
  / \
 1   3
```
**示例 1：**
```
输入: [1,6,3,2,5]
输出: false
```
**示例 2：**
```
输入: [1,3,2,6,5] 
输出: true
```
**提示：**
* `数组长度 <= 1000`
```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        if (postorder.empty()) return true;
        return dfs(postorder, 0, postorder.size()-1);
    }
    bool dfs(vector<int>& postorder, int l, int r) {
        if (l >= r) return true;
        int root = postorder[r];
        int mid = l;
        while (mid < r && postorder[mid] < root) mid++;
        for (int i = mid; i < r; ++i) {
            if (postorder[i] < root) return false;
        }
        return dfs(postorder,l,mid-1) && dfs(postorder,mid,r-1);
    }
};
```

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

### 剑指 Offer 35. 复杂链表的复制
请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。
**示例 1：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)
```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]] 
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```
**示例 2：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)
```
输入：head = [[1,1],[2,1]] 
输出：[[1,1],[2,1]]
```
**示例 3：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)
```
输入：head = [[3,null],[3,0],[3,null]] 
输出：[[3,null],[3,0],[3,null]]
```
**示例 4：**
```
输入：head = [] 
输出：[] 
解释：给定的链表为空（空指针），因此返回 null。
```
**提示：**
* `-10000 <= Node.val <= 10000`
* `Node.random 为空（null）或指向链表中的节点。`
* `节点数目不超过 1000 。`

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) return nullptr;
        unordered_map<Node*, Node*> mp;
        for (Node * ptr = head; ptr != nullptr; ptr=ptr->next) {
            mp[ptr] = new Node(ptr->val);
        }
        for (Node * ptr = head; ptr != nullptr; ptr=ptr->next) {
            mp[ptr]->next = mp[ptr->next];
            mp[ptr]->random = mp[ptr->random];
        }
        return mp[head];
    }
};
```

### 剑指 Offer 36. 二叉搜索树与双向链表
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。
为了让您更好地理解问题，以下面的二叉搜索树为例：
![](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)
我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。
下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。
![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)
特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    vector<Node *> ve;
    Node* treeToDoublyList(Node* root) {
        if (root == nullptr) return nullptr;
        Inorder(root);

        for (int i = 0; i < ve.size()-1; ++i) {
            ve[i+1]->left = ve[i];
            ve[i]->right = ve[i+1];
        }
        ve[0]->left = ve[ve.size()-1];
        ve[ve.size()-1]->right = ve[0];
        return ve[0];
    }
    void Inorder(Node* root) {
        if (root == nullptr) return;
        Inorder(root->left);
        ve.push_back(root);
        Inorder(root->right);
    }
};
```

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

### 剑指 Offer 38. 字符串的排列
输入一个字符串，打印出该字符串中字符的所有排列。
你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。
**示例:**
```
输入：s = "abc" 
输出：["abc","acb","bac","bca","cab","cba"]
```
**限制：**
* `1 <= s 的长度 <= 8`
```cpp
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> res;
        unordered_set<string> se;
        sort(s.begin(),s.end());
        do {
            if (!se.count(s)) {
                se.insert(s);
                res.push_back(s);
            }
        } while(next_permutation(s.begin(),s.end()));
        return res;
    }
};
```

### 剑指 Offer 39. 数组中出现次数超过一半的数字
数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。
**示例 1:**
```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2] 
输出: 2
```
**限制：**
`1 <= 数组长度 <= 50000`

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int len = nums.size();
        int res = 0;
        unordered_map<int,int> mp;
        for (int i = 0; i < len; ++i) {
            ++mp[nums[i]];
            if (mp[nums[i]] > len/2) {
                res = nums[i];
                break;
            }
        }
        return res;
    }
};
```

### 剑指 Offer 40. 最小的k个数
输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
**示例 1：**
```
输入：arr = [3,2,1], k = 2 
输出：[1,2] 或者 [2,1]
```
**示例 2：**
```
输入：arr = [0,1,2,1], k = 1 
输出：[0]
```
**限制：**
* `0 <= k <= arr.length <= 10000`
* `0 <= arr[i] <= 10000`

```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        stable_sort(arr.begin(),arr.end());
        vector<int> res;
        for (int i = 0; i < k; ++i) res.push_back(arr[i]);
        return res;
    }
};
```

### 剑指 Offer 41. 数据流中的中位数
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
例如，
[2,3,4] 的中位数是 3
[2,3] 的中位数是 (2 + 3) / 2 = 2.5
设计一个支持以下两种操作的数据结构：
* `void addNum(int num) - 从数据流中添加一个整数到数据结构中。`
* `double findMedian() - 返回目前所有元素的中位数。`
**示例 1：**
```
输入： ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"] [[],[1],[2],[],[3],[]] 
输出：[null,null,null,1.50000,null,2.00000]
```
**示例 2：**
```
输入： ["MedianFinder","addNum","findMedian","addNum","findMedian"] [[],[2],[],[3],[]] 
输出：[null,null,2.00000,null,2.50000]
```
限制：
* `最多会对 addNum、findMedia进行 50000 次调用。`

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    vector<int> ve;
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        ve.insert(lower_bound(ve.begin(),ve.end(),num),num);
    }
    
    double findMedian() {
        int len = ve.size();
        if (len % 2 == 0) return (ve[len/2-1] + ve[len/2]) / 2.0; 
        else return ve[len/2];
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

### 剑指 Offer 42. 连续子数组的最大和
输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
要求时间复杂度为O(n)。
**示例1:**
```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4] 
输出: 6 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```
**提示：**
`1 <= arr.length <= 10^5`
`-100 <= arr[i] <= 100`

```cpp
class Solution {
public:
    // dp[i] = max(a[i],dp[i-1]+a[i]);
    int maxSubArray(vector<int>& nums) {
        int res = nums[0], len = nums.size();
        vector<int> dp(len);
        dp[0] = nums[0];
        for (int i = 1; i < len; ++i) {
            dp[i] = max(nums[i],dp[i-1]+nums[i]);
            res = max(dp[i],res);
        }
        return res;
    }
};
```

### 剑指 Offer 43. 1～n整数中1出现的次数
输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。
例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。
**示例 1：**
```
输入：n = 12 
输出：5
```
**示例 2：**
```
输入：n = 13 
输出：6
```
**限制：**
`1 <= n < 2^31`

```cpp
class Solution {
public:
    int countDigitOne(int n) {
        long res = 0, tmp = n, count = 1;
        while (tmp) {
            if (tmp % 10 == 0) res += (tmp/10) * count;
            if (tmp % 10 == 1) res += (tmp/10) * count + (n%count) + 1;
            if (tmp % 10 > 1) res += ceil(tmp/10.0) * count;

            tmp /= 10;
            count *= 10;
        }
        return res;
    }
};
```

### 剑指 Offer 44. 数字序列中某一位的数字
数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。
请写一个函数，求任意第n位对应的数字。
**示例 1：**
```
输入：n = 3 
输出：3
```
**示例 2：**
```
输入：n = 11 
输出：0
```
**限制：**
* `0 <= n < 2^31`

```cpp
class Solution {
public:
    int findNthDigit(int n) {
        int digit = 1;  // n所在数字位数
        long start = 1;  // 数字范围开始的第一个数
        long count = 9;  // 占多少位
        while (n > count) {
            n -= count;
            digit++;
            start *= 10;
            count = digit * start * 9;
        }
        long num = start + (n-1) / digit;
        return to_string(num)[(n-1)%digit] - '0';
    }
};
```

### 剑指 Offer 45. 把数组排成最小的数
输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
**示例 1:**
```
输入: [10,2] 
输出: "102"
```
**示例 2:**
```
输入: [3,30,34,5,9] 
输出: "3033459"
```
**提示:**
* `0 < nums.length <= 100`
**说明:**
* `输出结果可能非常大，所以你需要返回一个字符串而不是整数`
* `拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0`

```cpp
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> strs;
        string res;
        for (int i = 0; i < nums.size(); ++i) {
            strs.push_back(to_string(nums[i]));
        }
        sort(strs.begin(),strs.end(),[](const string & a, const string & b){return a+b < b+a;});
        for (int i = 0; i < strs.size(); ++i) {
            res += strs[i];
        }
        return res;
    } 
};
```

### 剑指 Offer 46. 把数字翻译成字符串
给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。
**示例 1:**
```
输入: 12258 
输出: 5 
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```
**提示：**
* `0 <= num < 231`
```cpp
class Solution {
public:
    unordered_set<string> se;
    int translateNum(int num) {
        string s = to_string(num);
        dfs(s,0,"");
        return se.size();
    }
    void dfs(string & s, int pos, string tmp) {
        if (s.size() == pos) {
            se.insert(tmp);
            return;
        }
        // 1 to 1
        char ch = s[pos] - '0' + 'a';
        dfs(s,pos+1,tmp+ch);
        // 2 to 1
        if (s[pos] == '0' || s.size() - pos < 2) return;
        int num = (s[pos]-'0')*10 + (s[pos+1]-'0');
        if (num <= 25) {
            ch = 'a' + num;
            dfs(s,pos+2,tmp+ch);
        }
    }
};
```

### 剑指 Offer 47. 礼物的最大价值
在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
**示例 1:**
```
输入: [   
    [1,3,1],
    [1,5,1],
    [4,2,1] 
] 
输出: 12 
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```
**提示：**
* `0 < grid.length <= 200`
* `0 < grid[0].length <= 200`

```cpp
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> dp(n+1);
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                dp[j] = max(dp[j], dp[j-1]) + grid[i-1][j-1];
            }
        }
        return dp[n];
    }
};
```

### 剑指 Offer 48. 最长不含重复字符的子字符串
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。
**示例 1:**
```
输入: "abcabcbb" 
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
**示例 2:**
```
输入: "bbbbb" 
输出: 1 
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
**示例 3:**
```
输入: "pwwkew" 
输出: 3 
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。   请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
**提示：**
* `s.length <= 40000`

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0, tmp = 0, l = 0, r = 0;
        unordered_map<char,int> mp;
        while (r < s.size()) {
            while (r < s.size()) {
                mp[s[r]]++;
                if (mp[s[r]] == 1) tmp++;
                r++;
                if (mp[s[r-1]] > 1) break;
            }
            res = max(res,tmp);
            while (l < r) {
                mp[s[l]]--;
                if (mp[s[l]] == 0) tmp--;
                l++;
                if (mp[s[l-1]] == 1) break;
            }
        }
        return res;
    }
};
```

### 剑指 Offer 49. 丑数
我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
**示例:**
```
输入: n = 10 
输出: 12 
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```
**说明:**
* `1 是丑数。`
* `n 不超过1690。`

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> dp(n);
        dp[0] = 1;
        int i = 0, j = 0, k = 0;
        for (int x = 1; x < n; ++x) {
            int tmp = min(dp[i]*2,min(dp[j]*3,dp[k]*5));
            if (tmp == dp[i]*2) ++i;
            if (tmp == dp[j]*3) ++j;
            if (tmp == dp[k]*5) ++k;
            dp[x] = tmp;
        }
        return dp[n-1];
    }
};
```

### 剑指 Offer 50. 第一个只出现一次的字符
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
**示例:**
```
s = "abaccdeff" 
返回 "b" 
s = "" 
返回 " "
```
**限制：**
`0 <= s 的长度 <= 50000`
```cpp
class Solution {
public:
    int ch[26];
    char firstUniqChar(string s) {
        memset(ch,0,sizeof(ch));
        for (int i = 0; i < s.length(); ++i) ++ch[s[i]-'a'];
        for (int i = 0; i < s.length(); ++i) if (ch[s[i]-'a'] == 1) return s[i];
        return ' ';
    }
};
```

### 剑指 Offer 51. 数组中的逆序对
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
**示例 1:**
```
输入: [7,5,6,4] 
输出: 5
```
**限制：**
`0 <= 数组长度 <= 50000`
```cpp
class Solution {
public:
    int ans;
    void merge_sort(vector<int>& a, int l, int r) {
        if (l >= r) return;
        int mid = l+r>>1;
        merge_sort(a,l,mid);
        merge_sort(a,mid+1,r);

        vector<int> ve; 
        int i = l, j = mid+1;
        while (i <= mid && j <= r) {
            if (a[i] <= a[j]) ve.push_back(a[i++]);
            else {
                ans += (mid-i+1);
                ve.push_back(a[j++]);
            }
        }
        while (i <= mid) ve.push_back(a[i++]);
        while (j <= r) ve.push_back(a[j++]);
        
        for (int i = 0; i < ve.size(); ++i) {
            a[l+i] = ve[i];
        }
    }
    int reversePairs(vector<int>& nums) {
        ans = 0;
        merge_sort(nums,0,nums.size()-1);
        return ans;
    }
};
```

### 剑指 Offer 52. 两个链表的第一个公共节点
输入两个链表，找出它们的第一个公共节点。
如下面的两个链表：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
在节点 c1 开始相交。
**示例 1：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)
```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3 
输出：Reference of the node with value = 8 
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```
**示例 2：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)
```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1 
输出：Reference of the node with value = 2 
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```
**示例 3：**
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)
```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2 
输出：null 
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。 解释：这两个链表不相交，因此返回 null。
```
**注意：**
* `如果两个链表没有交点，返回 null.`
* `在返回结果后，两个链表仍须保持原有的结构。`
* `可假定整个链表结构中没有循环。`
* `程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。`

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode * ptr1 = headA, * ptr2 = headB;
        while (ptr1 != ptr2) {
            ptr1 = ptr1 == nullptr ? headB : ptr1->next;
            ptr2 = ptr2 == nullptr ? headA : ptr2->next;
        }
        return ptr1;
    }
};
```

### 剑指 Offer 53 - I. 在排序数组中查找数字 I
统计一个数字在排序数组中出现的次数。
**示例 1:**
```
输入: nums = [5,7,7,8,8,10], target = 8 
输出: 2
```
**示例 2:**
```
输入: nums = [5,7,7,8,8,10], target = 6 
输出: 0
```
**限制：**
`0 <= 数组长度 <= 50000`

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int res = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == target) ++res;
        }
        return res;
    }
};
```

### 剑指 Offer 53 - II. 0～n-1中缺失的数字
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
**示例 1:**
```
输入: [0,1,3] 
输出: 2
```
**示例 2:**
```
输入: [0,1,2,3,4,5,6,7,9] 
输出: 8
```
**限制：**
* `1 <= 数组长度 <= 10000`

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int l = 0, r = nums.size();
        while (l < r) {
            int mid = l+r>>1;
            if (nums[mid] == mid) l = mid+1;
            else r = mid;
        }
        if (l == nums.size()-1 && l == nums[l]) return l+1;
        return l;
    }
};
```

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

### 剑指 Offer 55 - I. 二叉树的深度
输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。
例如：
给定二叉树 [3,9,20,null,null,15,7]，
```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。
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
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + max(maxDepth(root->left),maxDepth(root->right));
    }
};
```

### 剑指 Offer 55 - II. 平衡二叉树
输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
**示例 1:**
给定二叉树 [3,9,20,null,null,15,7]
```
    3
   / \
  9  20
    /  \
   15   7
```
返回 true 。

**示例 2:**
给定二叉树 [1,2,2,3,3,null,null,4,4]
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
返回 false 。
**限制：**
* `1 <= 树的结点个数 <= 10000`

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
    bool isBalanced(TreeNode* root) {
        if (TreeHeight(root) == -1) return false;
        return true;
    }
    int TreeHeight(TreeNode* root) {
        if (root == nullptr) return 0;
        int LH = TreeHeight(root->left);
        int RH = TreeHeight(root->right);

        if (LH == -1 || RH == -1 || abs(LH-RH) > 1) return -1;
        return 1 + max(LH,RH);
    }
};
```

### 剑指 Offer 56 - I. 数组中数字出现的次数
一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
**示例 1：**
```
输入：nums = [4,1,4,6] 
输出：[1,6] 或 [6,1]
```
**示例 2：**
```
输入：nums = [1,2,10,4,1,4,3,3] 
输出：[2,10] 或 [10,2]
```
**限制：**
`2 <= nums.length <= 10000`
```cpp
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int tmp = 0, len = nums.size();
        for (int i = 0; i < len; ++i) {
            tmp ^= nums[i];
        }
        int bit = tmp&(-tmp);
        int a = 0;
        for (int i = 0; i < len; ++i) {
            if (bit & nums[i]) a ^= nums[i];
        }
        int b = tmp^a;
        return vector<int>({a,b});
    }
};
```

### 剑指 Offer 56 - II. 数组中数字出现的次数 II
在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。
**示例 1：**
```
输入：nums = [3,4,3,3] 
输出：4
```
**示例 2：**
```
输入：nums = [9,1,7,9,7,9,7]
输出：1
```
**限制：**

* `1 <= nums.length <= 10000`
* `1 <= nums[i] < 2^31`

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int,int> mp;
        int res = 0;
        for (int i = 0; i < nums.size(); ++i) {
            ++mp[nums[i]];
        }
        for (unordered_map<int,int>::iterator it = mp.begin(); it != mp.end(); ++it) {
            if (it->second == 1) {
                res = it->first;
                break;
            }
        }
        return res;
    }
};
```

### 剑指 Offer 57 - II. 和为s的连续正数序列
输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。
序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
**示例 1：**
```
输入：target = 9 
输出：[[2,3,4],[4,5]]
```
**示例 2：**
```
输入：target = 15 
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```
**限制：**
`1 <= target <= 10^5`
```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> res;
        vector<int> tmp;
        int l = 1, r = 1, sum = 0, lim = target/2;
        while (l <= lim) {
            if (sum < target) {
                sum += r;
                tmp.push_back(r);
                ++r;
            }
            else if (sum > target) {
                sum -= l;
                tmp.erase(tmp.begin());
                ++l;
            }
            else {
                res.push_back(tmp);
                sum -= l;
                tmp.erase(tmp.begin());
                ++l;
            }
        }
        return res;
    }
};
```

### 剑指 Offer 57. 和为s的两个数字
输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
**示例 1：**
```
输入：nums = [2,7,11,15], target = 9 
输出：[2,7] 或者 [7,2]
```
**示例 2：**
```
输入：nums = [10,26,30,31,47,60], target = 40 
输出：[10,30] 或者 [30,10]
```
**限制：**
* `1 <= nums.length <= 10^5`
* `1 <= nums[i] <= 10^6`
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        int l = 0, r = nums.size()-1;
        while (l < r) {
            int sum = nums[l]+nums[r];
            if (sum == target) {
                res.push_back(nums[l]);
                res.push_back(nums[r]);
                break;
            }
            if (sum > target) {
                --r;
            }
            else {
                ++l;
            }
        }
        return res;
    }
};
```

### 剑指 Offer 58 - I. 翻转单词顺序
输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。
**示例 1：**
```
输入: "the sky is blue" 
输出: "blue is sky the"
```
**示例 2：**
```
输入: "  hello world!  " 
输出: "world! hello" 解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```
**示例 3：**
```
输入: "a good   example" 
输出: "example good a" 
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```
**说明：**
* 无空格字符构成一个单词。
* 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
* 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

```cpp
class Solution {
public:
    string reverseWords(string s) {
        string tmp;
        vector<string> ve;
        s += ' ';
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == ' ') {
                if (!tmp.empty()) {
                    ve.push_back(tmp);
                    tmp.clear();
                }
                continue;
            }
            tmp += s[i];
        }
        string res;
        for (int i = ve.size()-1; i >= 0; --i) {
            res += ve[i];
            if (i != 0) res += ' ';
        }
        return res;
    }
};
```

### 剑指 Offer 58 - II. 左旋转字符串
字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。
**示例 1：**
```
输入: s = "abcdefg", k = 2 
输出: "cdefgab"
```
**示例 2：**
```
输入: s = "lrloseumgh", k = 6 
输出: "umghlrlose"
```
**限制：**
* `1 <= k < s.length <= 10000`

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        return s.substr(n)+s.substr(0,n);
    }
};
```

### 剑指 Offer 59 - I. 滑动窗口的最大值

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

**示例:**
```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**提示：**
你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        int len = nums.size();
        if (len == 0) return res;

        deque<int> dque;
        for (int i = 0; i < len; ++i) {
            while (!dque.empty() && i-dque.front()+1 > k) {
                dque.pop_front();
            }
            while (!dque.empty() && nums[dque.back()] <= nums[i]) {
                dque.pop_back();
            }
            dque.push_back(i);
            if (i >= k-1) res.push_back(nums[dque.front()]);
        }

        return res;
    }
};
```

### 剑指 Offer 59 - II. 队列的最大值
请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。
若队列为空，pop_front 和 max_value 需要返回 -1
**示例 1：**
```
输入: ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"] 
[[],[1],[2],[],[],[]] 
输出: [null,null,null,2,1,2]
```
**示例 2：**
```
输入: ["MaxQueue","pop_front","max_value"] [[],[],[]] 
输出: [null,-1,-1]
```
**限制：**
* `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
* `1 <= value <= 10^5`
```cpp
class MaxQueue {
public:
    queue<int> que;
    deque<int> mx_que;
    MaxQueue() {

    }
    
    int max_value() {
        if (que.empty()) return -1;
        return mx_que.front();
    }
    
    void push_back(int value) {
        que.push(value);

        while (!mx_que.empty() && mx_que.back() < value) {
            mx_que.pop_back();
        }
        mx_que.push_back(value);
    }
    
    int pop_front() {
        if (que.empty()) return -1;
        int res = que.front(); que.pop();
        if (res == mx_que.front()) mx_que.pop_front();
        return res;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```

### 剑指 Offer 60. n个骰子的点数
把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。
你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。
**示例 1:**
```
输入: 1 
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```
**示例 2:**
```
输入: 2 
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```
**限制：**
`1 <= n <= 11`
```cpp
class Solution {
public:
    vector<double> twoSum(int n) {
        vector<vector<int>> dp(n+1, vector<int>(n*6+1,0));

        for (int i = 1; i <= 6; ++i) dp[1][i] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = i; j <= 6*n; ++j) {
                for (int k = 1; k <= 6; ++k) {
                    if (j >= k) dp[i][j] += dp[i-1][j-k];
                }
            }
        }

        vector<double> res;
        double sum = pow(6,n);
        for (int i = n; i <= 6*n; ++i) {
            res.push_back(dp[n][i]/sum);
        }
        return res;
    }
};
```

### 剑指 Offer 61. 扑克牌中的顺子
从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
**示例 1:**
```
输入: [1,2,3,4,5] 
输出: True
```
**示例 2:**
```
输入: [0,0,1,2,5] 
输出: True
```
**限制：**
* `数组长度为 5`
* `数组的数取值为 [0, 13] .`
```cpp
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        int zero = 0, mi = 99, mx = 0;
        unordered_map<int,int> mp;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == 0) ++zero;
            else {
                mi = min(mi,nums[i]);
                mx = max(mx,nums[i]);
                ++mp[nums[i]];
                if (mp[nums[i]] > 1) return false;
            }
        }
        if (zero == 0) return mx-mi == 4;
        return mx-mi <= 4;
    }
};
```

### 剑指 Offer 62. 圆圈中最后剩下的数字
0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
**示例 1：**
```
输入: n = 5, m = 3 
输出: 3
```
**示例 2：**
```
输入: n = 10, m = 17 
输出: 2
```
**限制：**
* `1 <= n <= 10^5`
* `1 <= m <= 10^6`
```cpp
class Solution {
public:
    int lastRemaining(int n, int m) {
        int res = 0;
        for (int i = 2; i <= n; ++i) {
            res = (res + m) % i;
        }
        return res;
    }
};
```

### 剑指 Offer 63. 股票的最大利润
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？
**示例 1:**
```
输入: [7,1,5,3,6,4] 
输出: 5 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。 
注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```
**示例 2:**
```
输入: [7,6,4,3,1] 
输出: 0 
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
**限制：**
`0 <= 数组长度 <= 10^5`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for (int i = 1; i < prices.size(); ++i) {
            res = max(res,prices[i]-prices[i-1]);
            prices[i] = min(prices[i],prices[i-1]);
        }
        return res;
    }
};
```

### 剑指 Offer 64. 求1+2+…+n
求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
**示例 1：**
```
输入: n = 3 
输出: 6
```
**示例 2：**
```
输入: n = 9 
输出: 45
```
**限制：**
`1 <= n <= 10000`
```cpp
class Solution {
public:
    int sumNums(int n) {
        // && 的短路特性
        n && (n += sumNums(n-1));
        return n;
    }
};
```

### 剑指 Offer 65. 不用加减乘除做加法
写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
**示例:**
```
输入: a = 1, b = 1 
输出: 2
```
**提示：**
* `a, b 均可能是负数或 0`
* `结果不会溢出 32 位整数`

```cpp
class Solution {
public:
    int add(int a, int b) {
        if (b == 0) return a;
        return add(a^b, (unsigned int)(a&b) << 1);
    }
};
```

### 剑指 Offer 66. 构建乘积数组
给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 B 中的元素 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。
**示例:**
```
输入: [1,2,3,4,5] 
输出: [120,60,40,30,24]
```
**提示：**
* `所有元素乘积之和不会溢出 32 位整数`
* `a.length <= 100000`

```cpp
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int> res;
        int cur = 1, len = a.size();
        for (int i = 0; i < len; ++i) {
            res.push_back(cur);
            cur *= a[i];
        }
        cur = 1;
        for (int i = len-1; i >= 0; --i) {
            res[i] *= cur;
            cur *= a[i];
        }
        return res;
    }
};
```

### 剑指 Offer 67. 把字符串转换成整数
写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0。
说明：
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。
**示例 1:**
```
输入: "42" 
输出: 42
```
**示例 2:**
```
输入: " -42"
 输出: -42 
解释: 第一个非空白字符为 '-', 它是一个负号。 
我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```
**示例 3:**
```
输入: "4193 with words" 
输出: 4193 
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```
**示例 4:**
```
输入: "words and 987" 
输出: 0 
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。 因此无法执行有效的转换。
```
**示例 5:**
```
输入: "-91283472332" 
输出: -2147483648 
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。
因此返回 INT_MIN (−231) 。
```

```cpp
class Solution {
public:
    int strToInt(string str) {
        while (str[0] == ' ') str.erase(str.begin());
        if (!isdigit(str[0]) && str[0] != '+' && str[0] != '-') return 0;
        
        bool flag = false;  // 判断正负
        if (str[0] == '-') flag = true, str.erase(str.begin());
        else if (str[0] == '+') str.erase(str.begin());

        long res = 0;
        for (int i = 0; i < str.size(); ++i) {
            if (!isdigit(str[i])) break;
            res = res*10 + str[i]-'0';
            if (flag == false && res > INT_MAX) return INT_MAX;
            else if (flag == true && -res < INT_MIN) return INT_MIN;
        }
        return flag ? -res : res;
    }
};
```

### 剑指 Offer 68 - I. 二叉搜索树的最近公共祖先
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)
**示例 1:**
```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8 
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```
**示例 2:**
```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4 
输出: 2 
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```
**说明:**
* `所有节点的值都是唯一的。`
* `p、q 为不同节点且均存在于给定的二叉搜索树中。`

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // 都在左子树
        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        }
        // 都在右子树
        if (p->val > root->val && q->val > root->val) {
            return lowestCommonAncestor(root->right, p, q);
        }
        return root;
    }
};
```

### 剑指 Offer 68 - II. 二叉树的最近公共祖先
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
<br/>
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)
<br/>
**示例 1:**
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1 
输出: 3 
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```
**示例 2:**
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4 
输出: 5 
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```
**说明:**
* `所有节点的值都是唯一的。`
* `p、q 为不同节点且均存在于给定的二叉树中。`

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q) {
            return root;
        }
        TreeNode * left = lowestCommonAncestor(root->left, p, q);
        TreeNode * right = lowestCommonAncestor(root->right, p, q);
        
        // 一左一右
        if (left != nullptr && right != nullptr) {
            return root;
        }
        // 都在左
        else if (left != nullptr) {
            return left;
        }
        // 都在右
        else {
            return right;
        }
    }
};
```

