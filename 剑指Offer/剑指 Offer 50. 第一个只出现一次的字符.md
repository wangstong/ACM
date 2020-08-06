### 剑指 Offer 50. 第一个只出现一次的字符
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
**示例:**
```
s = "abaccdeff" 
返回 "b" 
s = "" 
返回 " "
```
**限制：**
`0 <= s 的长度 <= 50000`
```cpp
class Solution {
public:
    int ch[26];
    char firstUniqChar(string s) {
        memset(ch,0,sizeof(ch));
        for (int i = 0; i < s.length(); ++i) ++ch[s[i]-'a'];
        for (int i = 0; i < s.length(); ++i) if (ch[s[i]-'a'] == 1) return s[i];
        return ' ';
    }
};
```

