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