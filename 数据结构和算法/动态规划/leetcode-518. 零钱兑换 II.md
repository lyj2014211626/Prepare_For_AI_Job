- [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)
- [官方解法](https://leetcode-cn.com/problems/coin-change-2/solution/ling-qian-dui-huan-ii-by-leetcode/)
- [参考解法](https://www.cnblogs.com/grandyang/p/7669088.html)
- 解法一：完全背包问题
    + 还是要使用 DP 来做，首先来考虑最简单的情况，如果只有一个硬币的话，那么给定钱数的组成方式就最多有1种，就看此钱数能否整除该硬币值。当有两个硬币的话，组成某个钱数的方式就可能有多种，比如可能由每种硬币单独来组成，或者是两种硬币同时来组成，怎么量化呢？比如我们有两个硬币 [1,2]，钱数为5，那么钱数的5的组成方法是可以看作两部分组成，一种是由硬币1单独组成，那么仅有一种情况 (1+1+1+1+1)；另一种是由1和2共同组成，说明组成方法中至少需要有一个2，所以此时先取出一个硬币2，然后只要拼出钱数为3即可，这个3还是可以用硬币1和2来拼，所以就相当于求由硬币 [1,2] 组成的钱数为3的总方法。是不是不太好理解，多想想。这里需要一个二维的 dp 数组，其中 dp[i][j] 表示用前i个硬币组成钱数为j的不同组合方法，怎么算才不会重复，也不会漏掉呢？我们采用的方法是一个硬币一个硬币的增加，每增加一个硬币，都从1遍历到 amount，对于遍历到的当前钱数j，组成方法就是不加上当前硬币的拼法 dp[i-1][j]，还要加上，去掉当前硬币值的钱数的组成方法，当然钱数j要大于当前硬币值，状态转移方程也在上面的分析中得到了：
    + dp[i][j] = dp[i - 1][j] + (j >= coins[i - 1] ? dp[i][j - coins[i - 1]] : 0)
    ```C++
    class Solution {
    public:
        int change(int amount, vector<int>& coins) {
            int n = coins.size();
            vector<vector<int>> dp(n + 1, vector<int>(amount + 1, 0));
            dp[0][0] = 1;
            for(int i = 1; i <= n; ++i)
            {
                dp[i][0] = 1;
                for(int j = 1; j <= amount; ++j)
                {
                    if(j >= coins[i - 1])
                    {
                        dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
                    }
                    else
                        dp[i][j] = dp[i - 1][j];
                }
            }
            return dp[n][amount];
        }
    }; 
    ```
- 空间优化
    ```C++
    class Solution {
    public:
        int change(int amount, vector<int>& coins) {
            int n = coins.size();
            vector<int> dp(amount + 1, 0);
            dp[0] = 1;
            for(int i = 0; i < n; ++i)
            {
                for(int j = 1; j <= amount; ++j)
                {
                    if(j >= coins[i])
                        dp[j] += dp[j - coins[i]];
                }
            }
            return dp[amount];
        }
    };
    ```