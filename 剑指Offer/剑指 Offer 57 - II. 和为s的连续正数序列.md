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