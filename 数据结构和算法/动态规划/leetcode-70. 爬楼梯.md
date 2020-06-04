- [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4079165.html)
- [官方解法](https://leetcode-cn.com/problems/climbing-stairs/solution/pa-lou-ti-by-leetcode/)
- 解法一：递归
    + 实际上跟斐波那契数列非常相似，假设梯子有n层，那么如何爬到第n层呢，因为每次只能爬1或2步，那么爬到第n层的方法要么是从第 n-1 层一步上来的，要不就是从 n-2 层2步上来的，所以递推公式非常容易的就得出了：dp[n] = dp[n-1] + dp[n-2]。 由于斐波那契额数列的求解可以用递归，所以博主最先尝试了递归，拿到 OJ 上运行，显示 Time Limit Exceeded，就是说运行时间超了，因为递归计算了很多分支，效率很低，这里需要用动态规划 (Dynamic Programming) 来提高效率，代码如下：
    ```C++
    class Solution {
    public:
        int climbStairs(int n) {
            if (n <= 1) return 1;
            vector<int> dp(n);
            dp[0] = 1; dp[1] = 2;
            for (int i = 2; i < n; ++i) {
                dp[i] = dp[i - 1] + dp[i - 2];
            }
            return dp.back();
        }
    };
    ```
    + 优化空间版本
    ```C++
    class Solution {
    public:
        int climbStairs(int n) {
            if(n == 0)
                return 1;
            int fst = 0;
            int lst = 1;
            int res;
            for(int i = 1; i <= n; ++i)
            {
                res = fst + lst;
                fst = lst;
                lst = res;
            }
            return res;
        }
    };
    ```

- 自顶向下的递归
    + 虽然前面说过递归的写法会超时，但是只要加上记忆数组，那就不一样了，因为记忆数组可以保存计算过的结果，这样就不会存在重复计算了，大大的提高了运行效率，其实递归加记忆数组跟迭代的 DP 形式基本是大同小异的，参见代码如下：
    ```C++
    class Solution {
    public:
        int climbStairs(int n) {
            vector<int> memo(n + 1);
            return helper(n, memo);
        }
        int helper(int n, vector<int>& memo) {
            if (n <= 1) return 1;
            if (memo[n] > 0) return memo[n];
            return memo[n] = helper(n - 1, memo) + helper(n - 2, memo);
        }
    };
    ```