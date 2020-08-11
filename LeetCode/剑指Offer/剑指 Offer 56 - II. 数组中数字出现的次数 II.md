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

