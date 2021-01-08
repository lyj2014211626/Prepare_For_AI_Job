- [115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4294105.html)
- [官方解法](https://leetcode-cn.com/problems/distinct-subsequences/solution/dong-tai-gui-hua-by-powcai-5/)
- 解法一：动态规划
    + 看到有关字符串的子序列或者配准类的问题，首先应该考虑的就是用动态规划 Dynamic Programming 来求解，这个应成为条件反射。而所有 DP 问题的核心就是找出状态转移方程，想这道题就是递推一个二维的 dp 数组，其中 dp[i][j] 表示s中范围是 [0, i] 的子串中能组成t中范围是 [0, j] 的子串的子序列的个数。下面我们从题目中给的例子来分析
    ```C++
    class Solution {
    public:
        int numDistinct(string s, string t) {
            int m = s.size();
            int n = t.size();
            vector<vector<long long int>> dp(n + 1, vector<long long int>(m + 1, 0));
            for(int i = 0; i <= m; ++i)
                dp[0][i] = 1;
            for(int j = 1; j <= m; ++j)
            {
                for(int i = 1; i <= n; ++i)
                {
                    if(s[j - 1] == t[i - 1])
                    {
                        dp[i][j] = dp[i - 1][j - 1] + dp[i][j - 1];
                    }
                    else
                        dp[i][j] = dp[i][j - 1];
                }
            }
            return dp[n][m];
        }
    };
    ```