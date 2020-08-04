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