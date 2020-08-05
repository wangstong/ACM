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