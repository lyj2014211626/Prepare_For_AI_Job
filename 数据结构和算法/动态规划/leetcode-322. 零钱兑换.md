- [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)
- [参考解法](https://www.cnblogs.com/grandyang/p/5138186.html)
- [官方解法](https://leetcode-cn.com/problems/coin-change/solution/322-ling-qian-dui-huan-by-leetcode-solution/)
- 解法一：自底向上的解法
    + 如果大家刷题有一阵子了的，那么应该会知道，对于求极值问题，主要考虑动态规划 Dynamic Programming 来做，好处是保留了一些中间状态的计算值，可以避免大量的重复计算。我们维护一个一维动态数组 dp，其中 dp[i] 表示钱数为i时的最小硬币数的找零，注意由于数组是从0开始的，所以要多申请一位，数组大小为 amount+1，这样最终结果就可以保存在 dp[amount] 中了。初始化 dp[0] = 0，因为目标值若为0时，就不需要硬币了。其他值可以初始化是 amount+1，为啥呢？因为最小的硬币是1，所以 amount 最多需要 amount 个硬币，amount+1 也就相当于当前的最大值了，注意这里不能用整型最大值来初始化，因为在后面的状态转移方程有加1的操作，有可能会溢出，除非你先减个1，这样还不如直接用 amount+1 舒服呢。好，接下来就是要找状态转移方程了，没思路？不要紧！回归例子1，假设我取了一个值为5的硬币，那么由于目标值是 11，所以是不是假如我们知道 dp[6]，那么就知道了组成 11 的 dp 值了？所以更新 dp[i] 的方法就是遍历每个硬币，如果遍历到的硬币值小于i值（比如不能用值为5的硬币去更新 dp[3]）时，用 dp[i - coins[j]] + 1 来更新 dp[i]，所以状态转移方程为：
    + dp[i] = min(dp[i], dp[i - coins[j]] + 1);
    + 其中 coins[j] 为第j个硬币，而 i - coins[j] 为钱数i减去其中一个硬币的值，剩余的钱数在 dp 数组中找到值，然后加1和当前 dp 数组中的值做比较，取较小的那个更新 dp 数组。先来看迭代的写法如下所示：
    ```C++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            vector<int> dp(amount + 1, amount + 1);
            dp[0] = 0;      //很关键
            for(int i = 1; i <= amount; ++i)
            {
                for(int j = 0; j < coins.size(); ++j)
                {
                    if(coins[j] <= i)
                    {
                        dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                    }
                }
            }
            return (dp[amount] > amount) ? -1 : dp[amount];
        }
    };
    ```

- 递归：自顶向下
    + 迭代的 DP 解法有一个好基友，就是递归+记忆数组的解法，说其是递归形式的 DP 解法也没错，但博主比较喜欢说成是递归加记忆数组。其目的都是为了保存中间计算结果，避免大量的重复计算，从而提高运算效率，思路都一样，仅仅是写法有些区别：
    ```C++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            vector<int> dp(amount + 1, INT_MAX);
            dp[0] = 0;
            return help(coins, amount, dp);
        }
        int help(vector<int>& coins, int amount, vector<int>&dp)
        {
            if(amount < 0)
                return -1;
            if(dp[amount] != INT_MAX)
                return dp[amount];
            for(int i = 0; i < coins.size(); ++i)
            {
                int t = help(coins, amount - coins[i], dp);
                if(t >= 0)
                    dp[amount] = min(dp[amount], t + 1);
            }
            return dp[amount] = (dp[amount] == INT_MAX) ? -1 : dp[amount];
        }
    };
    ```