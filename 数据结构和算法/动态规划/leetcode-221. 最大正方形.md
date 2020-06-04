- [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4550604.html)
- [官方解法](https://leetcode-cn.com/problems/maximal-square/solution/zui-da-zheng-fang-xing-by-leetcode/)
- 解法一：动态规划
    + 我们还可以进一步的优化时间复杂度到 O(n2)，做法是使用 DP，建立一个二维 dp 数组，其中 dp[i][j] 表示到达 (i, j) 位置所能组成的最大正方形的边长。我们首先来考虑边界情况，也就是当i或j为0的情况，那么在首行或者首列中，必定有一个方向长度为1，那么就无法组成长度超过1的正方形，最多能组成长度为1的正方形，条件是当前位置为1。边界条件处理完了，再来看一般情况的递推公式怎么办，对于任意一点 dp[i][j]，由于该点是正方形的右下角，所以该点的右边，下边，右下边都不用考虑，关心的就是左边，上边，和左上边。这三个位置的dp值 suppose 都应该算好的，还有就是要知道一点，只有当前 (i, j) 位置为1，dp[i][j] 才有可能大于0，否则 dp[i][j] 一定为0。当 (i, j) 位置为1，此时要看 dp[i-1][j-1], dp[i][j-1]，和 dp[i-1][j] 这三个位置，我们找其中最小的值，并加上1，就是 dp[i][j] 的当前值了，这个并不难想，毕竟不能有0存在，所以只能取交集，最后再用 dp[i][j] 的值来更新结果 res 的值即可，参见代码如下：
    ```C++
    class Solution {
    public:
        int maximalSquare(vector<vector<char>>& matrix) {
            if (matrix.empty() || matrix[0].empty()) return 0;
            int m = matrix.size(), n = matrix[0].size(), res = 0;
            vector<vector<int>> dp(m, vector<int>(n, 0));
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (i == 0 || j == 0) dp[i][j] = matrix[i][j] - '0';  //考虑一行 或者一列得情况 因为这里不能参考i-1 j-1得情况
                    else if (matrix[i][j] == '1') {     //为0得情况 不用考虑
                        dp[i][j] = min(dp[i - 1][j - 1], min(dp[i][j - 1], dp[i - 1][j])) + 1;
                    }
                    res = max(res, dp[i][j]);
                }
            }
            return res * res;
        }
    };
    ```

    + 动态规划的空间优化
    + 用一维数组保存子问题的解
    + 下面这种解法进一步的优化了空间复杂度，此时只需要用一个一维数组就可以解决，为了处理边界情况，padding 了一位，所以 dp 的长度是 n+1，然后还需要一个变量 pre 来记录上一个层的 dp 值，我们更新的顺序是行优先，就是先往下遍历，用一个临时变量t保存当前 dp 值，然后看如果当前位置为1，则更新 dp[i] 为 dp[i], dp[i-1], 和 pre 三者之间的最小值，再加上1，来更新结果 res，如果当前位置为0，则重置当前 dp 值为0，因为只有一维数组，每个位置会被重复使用，参见代码如下：
    + **这个其实就是把原来的 dp 向右平移了一位，在 dp[0] 处填了一个0，然后从1开始遍历，这样就不用担心 dp[i-1] 会越界了**
    ```C++
    class Solution {
    public:
        int maximalSquare(vector<vector<char>>& matrix) {
            if(matrix.empty() || matrix[0].empty())
                return 0;
            int m = matrix.size();
            int n = matrix[0].size();
            vector<int> dp(n + 1, 0); //多申请一个空间
            int res = 0;
            int pre = 0;
            for(int i = 0; i < m; ++i)
            {
                for(int j = 1; j <= n; ++j)//列从下标1开始 
                {
                    int t = dp[j];
                    if(matrix[i][j - 1] == '1')//j的下标
                    {
                        dp[j] = min(min(pre, dp[j]), dp[j - 1]) + 1;
                        res = max(res, dp[j]);
                    }
                    else    //注意该处条件
                        dp[j] = 0;
                    pre = t;    //注意赋值上一个
                }
            }
            return res * res;
        }
    };
    ```