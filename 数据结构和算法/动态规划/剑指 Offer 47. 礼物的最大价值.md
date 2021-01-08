- [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)
- [参考解法](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/solution/mian-shi-ti-47-li-wu-de-zui-da-jie-zhi-dong-tai-gu/)
- 解法一：动态规划
    + 分第一行第一列，判断即可。
    ```C++
    class Solution {
    public:
        int maxValue(vector<vector<int>>& grid) {
            int m = grid.size();
            int n = grid[0].size();
            for(int i = 0; i < m; ++i)
            {
                for(int j = 0; j < n; ++j)
                {
                    if(i == 0 && j == 0)
                        continue;
                    else if(i == 0)
                        grid[i][j] += grid[i][j - 1];
                    else if(j == 0)
                        grid[i][j] += grid[i - 1][j];
                    else grid[i][j] += max(grid[i - 1][j], grid[i][j - 1]);
                }
            }
            return grid[m - 1][n - 1];
        }
    };
    ```

- 写法二
    ```C++
    class Solution {
    public int maxValue(int[][] grid) {
        int row = grid.length;
        int column = grid[0].length;
        //dp[i][j]表示从grid[0][0]到grid[i - 1][j - 1]时的最大价值
        int[][] dp = new int[row + 1][column + 1];
        for (int i = 1; i <= row; i++) {
            for (int j = 1; j <= column; j++) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i - 1][j - 1];
            }
        }
        return dp[row][column];
    }
}
    ```