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