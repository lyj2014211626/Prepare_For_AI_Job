- [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
- [参考解法](https://leetcode-cn.com/problems/longest-common-subsequence/solution/dong-tai-gui-hua-zhi-zui-chang-gong-gong-zi-xu-lie/)
- 解法一：简单的动态规划
    + 直接用二位数组。
    + 这里要访问i - 1和j - 1。因此，从1：m。
    ```C++
    class Solution {
    public:
        int longestCommonSubsequence(string text1, string text2) {
            int m = text1.size();
            int n = text2.size();
            vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
            for(int i = 1; i <= m; ++i)
            {
                for(int j = 1; j <= n; ++j)
                {
                    if(text1[i - 1] == text2[j - 1])
                        dp[i][j] = dp[i - 1][j - 1] + 1;
                    else
                        dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
            return dp[m][n];
        }
    };
    ```
- 优化
    + 二位动态数组用二个数组进行表示
    ```C++
    class Solution {
    public:
        int longestCommonSubsequence(string text1, string text2) {
            int m = text1.size();
            int n = text2.size();
            vector<vector<int>> dp(2, vector<int>(n + 1, 0));
            int pre = 0, cur = 1;
            for(int i = 1; i <= m; ++i)
            {
                for(int j = 1; j <= n; ++j)
                {
                    if(text1[i - 1] == text2[j - 1])
                        dp[cur][j] = dp[pre][j - 1] + 1;
                    else
                        dp[cur][j] = max(dp[pre][j], dp[cur][j - 1]);
                }
                swap(pre, cur);
            }
            return dp[pre][n];
        }
    };
    ```

- 继续优化
    + 用以为数组进行表示。
    ```C++
    class Solution {
    public:
        int longestCommonSubsequence(string text1, string text2) {
            int m = text1.size();
            int n = text2.size();
            vector<int> dp(n + 1, 0);
            int pre = 0, cur = 1;
            int old = 0;
            int t = 0;
            for(int i = 1; i <= m; ++i)
            {
                old = 0;
                for(int j = 1; j <= n; ++j)
                {
                    t = dp[j];//代表dp[i - 1][j]的值
                    if(text1[i - 1] == text2[j - 1])
                        dp[j] = old + 1;//代表dp[i-1][j-1]+1
                    else
                        dp[j] = max(dp[j], dp[j - 1]);
                    old = t;//保留dp[i-1][j]的值留给下一步求解最优解时使
                }
                
            }
            return dp[n];
        }
    };
    ```