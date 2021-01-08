- [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4800552.html)
- [官方解法](https://leetcode-cn.com/problems/perfect-squares/solution/hua-jie-suan-fa-279-wan-quan-ping-fang-shu-by-guan/)
- 动态规划
    + 标签：动态规划
    + 首先初始化长度为n+1的数组dp，每个位置都为0
    + 如果n为0，则结果为0
    + 对数组进行遍历，下标为i，每次都将当前数字先更新为最大的结果，即dp[i]=i，比如i=4，最坏结果为4=1+1+1+1即为4个数字
    + 动态转移方程为：dp[i] = MIN(dp[i], dp[i - j * j] + 1)，i表示当前数字，j*j表示平方数
    + 时间复杂度：O(n*sqrt(n))，sqrt为平方根
    ```C++
    class Solution {
    public:
        int numSquares(int n) {
            vector<int> dp(n + 1, 0);
            for(int i = 1; i <= n; ++i)
            {
                dp[i] = i;
                for(int j = 1; i - j * j >= 0; ++j)
                {
                    dp[i] = min(dp[i], dp[i - j * j] + 1);
                }
            }
            return dp[n];
        }
    };
    ```