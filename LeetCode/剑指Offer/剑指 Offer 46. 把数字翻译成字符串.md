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

