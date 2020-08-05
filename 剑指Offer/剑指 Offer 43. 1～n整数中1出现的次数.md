### 剑指 Offer 43. 1～n整数中1出现的次数
输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。
例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。
**示例 1：**
```
输入：n = 12 
输出：5
```
**示例 2：**
```
输入：n = 13 
输出：6
```
**限制：**
`1 <= n < 2^31`

```cpp
class Solution {
public:
    int countDigitOne(int n) {
        long res = 0, tmp = n, count = 1;
        while (tmp) {
            if (tmp % 10 == 0) res += (tmp/10) * count;
            if (tmp % 10 == 1) res += (tmp/10) * count + (n%count) + 1;
            if (tmp % 10 > 1) res += ceil(tmp/10.0) * count;

            tmp /= 10;
            count *= 10;
        }
        return res;
    }
};
```