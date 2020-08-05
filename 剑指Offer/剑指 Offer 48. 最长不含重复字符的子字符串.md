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