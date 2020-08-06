### 剑指 Offer 14- II. 剪绳子 II
给你一根长度为` n `的绳子，请把绳子剪成整数长度的` m `段（m、n都是整数，n>1并且m>1），每段绳子的长度记为` k[0],k[1]...k[m - 1] `。请问` k[0]*k[1]*...*k[m - 1] `可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

**示例 1：**
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```
**示例 2:**
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

**提示：**

* `2 <= n <= 1000`

```cpp
class Solution {
public:
    long long qmul(long long a, long long b, long long p = 1000000007) {
        long long res = 0;
        while (b) {
            if (b & 1) res = (res+a)%p;
            a = (a+a)%p;
            b >>= 1;
        }
        return res%p;
    }
    long long qpow(long long a, long long b, long long p = 1000000007) {
        long long res = 1;
        while (b) {
            if (b & 1) res = res*a%p;
            a = a*a%p;
            b >>= 1;
        }
        return res%p;
    }
    int cuttingRope(int n) {
        if (n == 2) return 1;
        else if (n == 3) return 2;

        if (n % 3 == 0) return qpow(3,n/3);
        else if (n % 3 == 1) return qmul(qpow(3,n/3-1),4);
        else return qmul(qpow(3,n/3),2);
    }
};
```

