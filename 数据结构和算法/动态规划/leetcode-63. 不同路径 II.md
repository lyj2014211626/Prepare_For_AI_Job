- [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)
- [参考博客](https://github.com/grandyang/leetcode/issues/63)
- [官方解答](https://leetcode-cn.com/problems/unique-paths-ii/solution/bu-tong-lu-jing-ii-by-leetcode/)
- 解法一：动态规划
    + 这道题是之前那道 不同路径 的延伸，在路径中加了一些障碍物，还是用动态规划 Dynamic Programming 来解，使用一个二维的 **dp 数组，大小为 (m+1) x (n+1)，这里的 dp[i][j] 表示到达 (i-1, j-1) 位置的不同路径的数量**，那么i和j需要更新的范围就是 [1, m] 和 [1, n]。状态转移方程跟之前那道题是一样的，因为每个位置只能由其上面和左面的位置移动而来，所以也是由其上面和左边的 dp 值相加来更新当前的 dp 值，如下所示：
    + dp[i][j] = dp[i-1][j] + dp[i][j-1]
    + 这里就能看出来初始化 dp数组的大小为 (m+1) x (n+1)，是为了 handle 边缘情况，**当i或j为0时，减1可能会出错。当某个位置是障碍物时，其 dp 值为0，直接跳过该位置即可**。这里还需要初始化 dp 数组的某个值，使得其能正常累加。当起点不是障碍物时，其 dp 值应该为1，即dp[1][1] = 1，由于其是由 dp[0][1] + dp[1][0] 更新而来，所以二者中任意一个初始化为1即可。由于之后 LeetCode 更新了这道题的 test case，使得使用 int 型的 dp 数组会有溢出的错误，所以改为使用 long 型的数组来避免 overflow，代码如下：
    + C++代码如下：
    ```
    class Solution {
    public:
        int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
            if(obstacleGrid.empty() || obstacleGrid[0].empty() || obstacleGrid[0][0])//边界的条件的处理很重要 一定要进行判断
                return 0;
            int m = obstacleGrid.size();
            int n = obstacleGrid[0].size();
            vector<vector<long long>> dp(m + 1, vector<long long>(n + 1, 0));
            dp[0][1] = 1;
            for(int i = 1; i <= m; ++i)
            {
                for(int j = 1; j <= n; ++j)
                {
                    if(obstacleGrid[i - 1][j - 1])
                        continue;//dp[i][j] = 0;
                    else
                        dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
            return dp[m][n];
        }
    };
    ```
    
    + C++简化版 这里dp用一维向量表示，可以说是很巧妙啦
    ```
    class Solution {
    public:
        int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
            if(obstacleGrid.empty() || obstacleGrid[0].empty() || obstacleGrid[0][0])
                return 0;
            int m = obstacleGrid.size();
            int n = obstacleGrid[0].size();
            vector<long long> dp(n, 0);
            dp[0] = 1;//初始化
            for(int i = 0; i < m; ++i)
            {
                for(int j = 0; j < n; ++j)
                {
                    if(obstacleGrid[i][j])
                        dp[j] = 0;
                    else
                        if(j > 0)//这里很重要
                            dp[j] += dp[j - 1];
                }
            }
            return dp[n - 1];
        }
    };
    ```