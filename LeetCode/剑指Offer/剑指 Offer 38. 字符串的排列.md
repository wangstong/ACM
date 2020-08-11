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

