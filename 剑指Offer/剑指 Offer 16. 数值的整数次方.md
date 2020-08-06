### 剑指 Offer 16. 数值的整数次方
实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

 

**示例 1:**
```
输入: 2.00000, 10
输出: 1024.00000
```
**示例 2:**
```
输入: 2.10000, 3
输出: 9.26100
```
**示例 3:**
```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```

**说明:**

* `-100.0 < x < 100.0`
* `n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。`


```cpp
class Solution {
public:
    double qmul1(double a, int b) {
        double res = 1.0;
        while (b) {
            if (b & 1) res = res*a;
            a = a*a;
            b >>= 1;
        }
        return res;
    }
    double qmul2(double a, long b) {
        double res = 1.0;
        b = -b;
        while (b) {
            if (b & 1) res = res/a;
            a = a*a;
            b >>= 1;
        }
        return res;
    }
    double myPow(double x, int n) {
        return n >= 0 ? qmul1(x,n) : qmul2(x,n);
    }
};
```

