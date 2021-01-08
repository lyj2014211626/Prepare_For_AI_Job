- [剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)
- [参考解法](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/nge-tou-zi-de-dian-shu-dong-tai-gui-hua-ji-qi-yo-3/)
- 解法一：动态规划
    + 我们用 dp[n][j]dp[n][j] 来表示最后一个阶段点数 jj 出现的次数。
    ```C++
    class Solution {
    public:
        vector<double> twoSum(int n) {
            int dp[15][70];
            memset(dp, 0, sizeof(dp));
            for(int i = 1; i <= 6; ++i)
                dp[1][i] = 1;
            for(int i = 2; i <= n; ++i)
            {
                for(int j = i; j <= 6*i; ++j)
                {
                    for(int k = 1; k <= 6; ++k)
                    {
                        if(j - k <= 0)
                        {
                            break;
                        }
                        dp[i][j] += dp[i - 1][j - k];
                    }
                }
            }
            int all = pow(6, n);
            vector<double> res;
            for(int i = n; i <= 6 * n; ++i)
                res.push_back(dp[n][i] * 1.0 / all);
            return res;
        }
    };
    ```

- 空间优化
    + 我们知道，每个阶段的状态都只和它前一阶段的状态有关，因此我们不需要用额外的一维来保存所有阶段。
    + 用一维数组来保存一个阶段的状态，然后对下一个阶段可能出现的点数 jj 从大到小遍历，实现一个阶段到下一阶段的转换。
    ```C++
    class Solution {
    public:
        vector<double> twoSum(int n) {
            int dp[70];
            memset(dp, 0, sizeof(dp));
            for(int i = 1; i <= 6; ++i)
                dp[i] = 1;
            for(int i = 2; i <= n; ++i)
            {
                for(int j = 6*i; j >= i; --j)
                {
                    dp[j] = 0;
                    for(int k = 1; k <= 6; ++k)
                    {
                        if(j - k < i - 1)
                        {
                            break;
                        }
                        dp[j] += dp[j - k];
                    }
                }
            }
            int all = pow(6, n);
            vector<double> res;
            for(int i = n; i <= 6 * n; ++i)
                res.push_back(dp[i] * 1.0 / all);
            return res;
        }
    };
    ```