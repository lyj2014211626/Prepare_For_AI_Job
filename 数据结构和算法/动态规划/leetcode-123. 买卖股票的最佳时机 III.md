- [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4281975.html)
- [官方解法](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)
- 解法一：动态规划
    + 而这道是要求最多交易两次，找到最大利润，还是需要用动态规划Dynamic Programming来解，而这里我们需要两个递推公式来分别更新两个变量local和global，参见网友[Code Ganker的博客](https://blog.csdn.net/linhuanmars/article/details/23236995)，我们其实可以求至少k次交易的最大利润，找到通解后可以设定 k = 2，即为本题的解答。我们定义local[i][j]为在到达第i天时最多可进行j次交易并且最后一次交易在最后一天卖出的最大利润，此为局部最优。然后我们定义global[i][j]为在到达第i天时最多可进行j次交易的最大利润，此为全局最优。它们的递推式为：

    + local[i][j] = max(global[i - 1][j - 1] + max(diff, 0), local[i - 1][j] + diff)

    + global[i][j] = max(local[i][j], global[i - 1][j])

    + 其中局部最优值是比较前一天并少交易一次的全局最优加上大于0的差值，和前一天的局部最优加上差值中取较大值，而全局最优比较局部最优和前一天的全局最优，代码如下：
    ```C++
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            if(prices.empty())
                return 0;
            int res = 0;
            int n = prices.size();
            vector<vector<int>> g(n, vector<int>(3, 0));
            vector<vector<int>> l(n, vector<int>(3, 0));
            for(int i = 1; i < n; ++i)  //  注意下标
            {
                int diff = prices[i] - prices[i - 1];
                for(int j = 1; j <= 2 ; ++j)
                {
                    l[i][j] = max(g[i - 1][j - 1] + max(0, diff), l[i - 1][j] + diff);  //注意逻辑
                    g[i][j] = max(g[i - 1][j], l[i][j]);
                }
            }
            return g[n - 1][2];
        }
    };
    ```

- 空间优化的动态规划
    + 下面这种解法用一维数组来代替二维数组，可以极大的节省了空间，**由于覆盖的顺序关系，我们需要j从2到1，这样可以取到正确的g[j-1]值**，而非已经被覆盖过的值，参见代码如下：
    ```C++
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            if(prices.empty())
                return 0;
            int res = 0;
            int n = prices.size();
            vector<int> g(3, 0);
            vector<int> l(3, 0);
            for(int i = 0; i < n - 1; ++i)
            {
                int diff = prices[i + 1] - prices[i];
                for(int j = 2; j >= 1 ; --j)    //  顺序变化
                {
                    l[j] = max(g[j - 1] + max(0, diff), l[j] + diff);
                    g[j] = max(g[j], l[j]);
                }
            }
            return g[2];
        }
    };
    ```