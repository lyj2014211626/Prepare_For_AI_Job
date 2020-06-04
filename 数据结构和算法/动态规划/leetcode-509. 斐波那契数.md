- [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)
- [参考博客](https://www.cnblogs.com/grandyang/p/10306787.html)
- [官方解法](https://leetcode-cn.com/problems/fibonacci-number/solution/dong-tai-gui-hua-tao-lu-xiang-jie-by-labuladong/)
- 解法一：递归
    + 递归写法虽然简单，但是并不高效，因为有大量的重复计算。时间复杂度是O(2^n)级别，显然不是最优的解法。
    ```C++
    class Solution {
    public:
        int fib(int N) {
            if (N <= 1) return N;
            return fib(N - 1) + fib(N - 2);
        }
    };
    ```

- 解法二：动态规划（自顶向下的带备忘录的解法）
    + 即然耗时的原因是重复计算，那么我们可以造一个「备忘录」，每次算出某个子问题的答案后别急着返回，先记到「备忘录」里再返回；每次遇到一个子问题先去「备忘录」里查一查，如果发现之前已经解决过这个问题了，直接把答案拿出来用，不要再耗时去计算了。
    + 一般使用一个数组充当这个「备忘录」，当然你也可以使用哈希表（字典），思想都是一样的。
    ```C++
    class Solution {
    public:
        int fib(int N) {
            if(N <= 1)
                return N;
            vector<int> memo(N + 1, 0);
            return cursion(N, memo);
        }
        int cursion(int N, vector<int> &memo)
        {
            if(N == 1 || N == 2)
                return 1;
            if(memo[N] != 0)
                return memo[N];
            memo[N] = cursion(N - 1, memo) + cursion(N - 2, memo);
            return memo[N];
        }
    };
    ```

- 解法三：动态规划(自底向上)
    + Dynamic Programming 来做，建立一个大小为 N+1 的 dp 数组，其中 dp[i] 为位置i上的数字，先初始化前两个分别为0和1，然后就可以开始更新整个数组了，状态转移方程就是斐波那契数组的性质，最后返回 dp[N] 即可，参见代码如下：
    ```C++
    class Solution {
    public:
        int fib(int N) {
            vector<int> dp(N + 1);
            dp[0] = 0; dp[1] = 1;
            for (int i = 2; i <= N; ++i) {
                dp[i] = dp[i - 1] + dp[i - 2];
            }
            return dp[N];
        }
    };
    ```

- 解法四：动态规划(空间优化)
    + 由于当前数字只跟前两个数字有关，所以不需要保存整个数组，而是只需要保存前两个数字就行了。
    ```C++
    class Solution {
    public:
        int fib(int N) {
            if(N <= 1)
                return N;
            int fst = 0;
            int lst = 1;
            int res = 0;
            for(int i = 2; i <=  N; ++i)
            {
                res = fst + lst;
                fst = lst;
                lst = res;
            }
            return res;
        }
    };
    ```