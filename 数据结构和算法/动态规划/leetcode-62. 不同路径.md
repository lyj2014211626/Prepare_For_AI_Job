- [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)
- [参考博客](https://github.com/grandyang/leetcode/issues/62)
- [官方解法](https://leetcode-cn.com/problems/unique-paths/solution/dong-tai-gui-hua-by-powcai-2/)
- 解法一：动态规划
    + 原来这跟之前那道 Climbing Stairs 很类似，那道题是说可以每次能爬一格或两格，问到达顶部的所有不同爬法的个数。而这道题是每次可以向下走或者向右走，求到达最右下角的所有不同走法的个数。那么跟爬梯子问题一样，需要用动态规划 Dynamic Programming 来解，可以维护一个二维数组 dp，其中 dp[i][j] 表示到当前位置不同的走法的个数，然后可以得到状态转移方程为:  dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    + C++版本
    ```
    class Solution {
    public:
        int uniquePaths(int m, int n) {
            vector<vector<int> > dp (m + 1, vector<int>(n + 1, 0));
            dp[0][1] = 1;//初始化 这里假定m n都是大于0的
            for(int i = 1; i <= m; ++i)
            {
                for(int j = 1; j <= n; ++j)
                {
                    dp[i][j] = dp[i][j - 1] + dp[i - 1][j];
                }
            }
            return dp[m][n];
        }
    };
    ```

    + 改进，为了节省空间，其实可以用一维数组的。这种优化的方式真是太叼了，因为只要m = 1或者n = 1,就是初始化的结果。然后剩下的就从下标1开始，很巧妙了可以说是。
    ```
    class Solution {
    public:
        int uniquePaths(int m, int n) {
            vector<int> dp(n, 1);
            for(int i = 1; i < m; ++i)//从下标1开始
            {
                for(int j = 1; j < n; ++j)//从下标1开始
                    dp[j] += dp[j - 1];
            }
            return dp[n - 1];
        }
    };
    ```