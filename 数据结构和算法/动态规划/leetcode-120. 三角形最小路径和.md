- [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4286274.html)
- [官方解法](https://leetcode-cn.com/problems/triangle/solution/san-jiao-xing-zui-xiao-lu-jing-he-by-leetcode-solu/)
- 解法一：动态规划
    + 这道题给了我们一个二维数组组成的三角形，让我们寻找一条自上而下的路径，使得路径和最短。那么那道题后还是先考虑下暴力破解，我们可以发现如果要遍历所有的路径的话，那可以是阶乘级的时间复杂度啊，OJ必灭之，趁早断了念想比较好。必须要优化时间复杂度啊，题目中给的例子很容易把人带偏，让人误以为贪婪算法可以解题，因为看题例子中的红色数组，在根数字2的下方选小的数字3，在3的下方选小的数字5，在5的下方选小的数字1，每次只要选下一层相邻的两个数字中较小的一个，似乎就能得到答案了。其实是不对的，贪婪算法可以带到了局部最小，但不能保证每次都带到全局最小，很有可能在其他的分支的底层的数字突然变的超级小，但是贪婪算法已经将其他所有分支剪掉了。所以为了保证我们能得到全局最小，动态规划Dynamic Programming还是不二之选啊。其实这道题和 Dungeon Game 非常的类似，都是用DP来求解的问题。那么其实我们可以不新建dp数组，而是直接复用triangle数组，我们希望一层一层的累加下来，从而使得 triangle[i][j] 是从最顶层到 (i, j) 位置的最小路径和，那么我们如何得到状态转移方程呢？其实也不难，因为每个结点能往下走的只有跟它相邻的两个数字，那么每个位置 (i, j) 也就只能从上层跟它相邻的两个位置过来，也就是 (i-1, j-1) 和 (i-1, j) 这两个位置，那么状态转移方程为：
    + triangle[i][j] = min(triangle[i - 1][j - 1], triangle[i - 1][j])
    + 我们从第二行开始更新，注意两边的数字直接赋值上一行的边界值，那么最终我们只要在最底层找出值最小的数字，就是全局最小的路径和啦，代码如下：
    ```C++
    class Solution {
    public:
        int minimumTotal(vector<vector<int>>& triangle) {
            if(triangle.empty() || triangle[0].empty())
                return 0;
            int min_num = INT_MAX;
            int m = triangle.size();
            int n = triangle[m - 1].size();
            for(int i = 1; i < m; ++i)
            {
                for(int j = 0; j < triangle[i].size(); ++j)
                {
                    if(j == 0)
                        triangle[i][j] += triangle[i - 1][j];
                    else if( j == triangle[i].size() - 1)
                        triangle[i][j] += triangle[i - 1][j - 1];
                    else
                    {
                        triangle[i][j] += min(triangle[i - 1][j - 1], triangle[i - 1][j]);
                    }
                }
            }
            for(int i = 0; i < n; ++i)
            {
                min_num = min(min_num, triangle[m - 1][i]);
            }
            return min_num;
        }
    };
    ```

- 优化
    + 自底向上
    ```C++
    class Solution {
    public:
        int minimumTotal(vector<vector<int>>& triangle) {
            if(triangle.empty() || triangle[0].empty())
                return 0;
            vector<int> dp(triangle.back());
            for(int i =  triangle.size() - 2; i >= 0; --i)
            {
                for(int j = 0; j <= i; ++j)
                {
                    dp[j] = min(dp[j], dp[j + 1]) + triangle[i][j];
                }
            }
            return dp[0];
        }
    };
    ```