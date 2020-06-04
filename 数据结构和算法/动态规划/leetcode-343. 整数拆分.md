- [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)
- [参考博客](https://www.cnblogs.com/grandyang/p/5411919.html)
- [官方解法](https://leetcode-cn.com/problems/integer-break/solution/343-zheng-shu-chai-fen-tan-xin-by-jyd/)
- 解法一：动态规划
    + 这道题给了我们一个正整数n，让拆分成至少两个正整数之和，使其乘积最大。最简单粗暴的方法自然是检查所有情况了，但是拆分方法那么多，怎么才能保证能拆分出所有的情况呢？当前的拆分方法需要用到之前的拆分值，这种重现关系就很适合动态规划 Dynamic Programming 来做，我们使用一个一维数组 dp，其中 dp[i] 表示数字i拆分为至少两个正整数之和的最大乘积，数组大小为 n+1，值均初始化为1，因为正整数的乘积不会小于1。可以从3开始遍历，因为n是从2开始的，而2只能拆分为两个1，乘积还是1。i从3遍历到n，对于每个i，需要遍历所有小于i的数字，因为这些都是潜在的拆分情况，对于任意小于i的数字j，**首先计算拆分为两个数字的乘积，即j乘以 i-j，然后是拆分为多个数字的情况，这里就要用到 dp[i-j] 了，这个值表示数字 i-j 任意拆分可得到的最大乘积**，再乘以j就是数字i可拆分得到的乘积，取二者的较大值来更新 dp[i]，最后返回 dp[n] 即可，参见代码如下：
    ```C++
    class Solution {
    public:
        int integerBreak(int n) {
            vector<int> dp(n + 1, 1);
            for(int i = 3; i <= n; ++i)
            {
                for(int j = 1; j < i; ++j)
                {
                    dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]));//关键
                }
            }
            return dp[n];
        }
    };
    ```

- 贪心算法
    + 题目提示中让用 O(n) 的时间复杂度来解题，而且告诉我们找7到 10 之间的规律，那么我们一点一点的来分析：
    + 正整数从1开始，但是1不能拆分成两个正整数之和，所以不能当输入。

        那么2只能拆成 1+1，所以乘积也为1。

        数字3可以拆分成 2+1 或 1+1+1，显然第一种拆分方法乘积大为2。

        数字4拆成 2+2，乘积最大，为4。

        数字5拆成 3+2，乘积最大，为6。

        数字6拆成 3+3，乘积最大，为9。

        数字7拆为 3+4，乘积最大，为 12。

        数字8拆为 3+3+2，乘积最大，为 18。

        数字9拆为 3+3+3，乘积最大，为 27。

        数字10拆为 3+3+4，乘积最大，为 36。

        ....

        那么通过观察上面的规律，我们可以看出从5开始，数字都需要先拆出所有的3，一直拆到剩下一个数为2或者4，因为剩4就不用再拆了，拆成两个2和不拆没有意义，而且4不能拆出一个3剩一个1，这样会比拆成 2+2 的乘积小
    ```C++
    class Solution {
    public:
        int integerBreak(int n) {
            if(n == 2 || n == 3)
                return n - 1;
            int res = 1;
            while(n > 4)
            {
                n -= 3;
                res *= 3;
            }
            return res * n;
        }
    };
    ```

    + 改进版
    + 不再使用 while 循环了，而是直接分别算出能拆出3的个数和最后剩下的余数2或者4，然后直接相乘得到结果
    ```C++
    class Solution {
    public:
        int integerBreak(int n) {
            if(n == 2 || n == 3)
                return n - 1;
            if(n == 4)
                return n;
            n -= 5;
            return pow(3, (n / 3 + 1)) * (n % 3 + 2);
        }
    };
    ```
    + 方便理解版本
    + 数尽可能的划分成3，剩下余数分成0， 1， 2三种情况进行讨论即可。
    ```C++
    class Solution {
    public:
        int integerBreak(int n) {
            if(n == 1)
                return 1;
            if(n <= 3)
                return n - 1;
            if(n == 4)
                return n;
            int p = n / 3;
            int e = n % 3;
            int exp = 0;
            if(e == 0)
                return pow(3, p);
            else if(e == 1)
                return pow(3, p - 1) * 4;
            else return pow(3, p) * 2;
        }
    };
    ```