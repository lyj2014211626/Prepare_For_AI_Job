- [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii
- [参考博客](https://www.cnblogs.com/grandyang/p/4280803.html)
- [官方解法](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-leetcode/)
- 解法一：
    + 这道题由于可以无限次买入和卖出。我们都知道炒股想挣钱当然是低价买入高价抛出，那么这里我们只需要从第二天开始，如果当前价格比之前价格高，则把差值加入利润中，因为我们可以昨天买入，今日卖出，若明日价更高的话，还可以今日买入，明日再抛出。以此类推，遍历完整个数组后即可求得最大利润。代码如下：
    ```C++
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            int res = 0;
            for(int i = 0; i < prices.size() - 1; ++i)
            {
                if(prices[i] < prices[i + 1])
                    res += prices[i + 1] - prices[i];
            }
            return res;
        }
    };
    ```