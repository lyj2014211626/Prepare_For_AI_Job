- [233. 数字 1 的个数](https://leetcode-cn.com/problems/number-of-digit-one/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4629032.html)
```C++
    class Solution {
    public:
        int countDigitOne(int n) {
            long long res = 0, a = 1, b = 1;
            while (n > 0) {
                res += (n + 8) / 10 * a + (n % 10 == 1) * b;
                b += n % 10 * a;
                a *= 10;
                n /= 10;
            }
            return res;
        }
    };
```