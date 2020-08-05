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