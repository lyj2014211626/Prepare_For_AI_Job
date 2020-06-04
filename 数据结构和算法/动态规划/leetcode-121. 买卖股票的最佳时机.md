- [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4280131.html)
- [官方解法](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/121-mai-mai-gu-piao-de-zui-jia-shi-ji-by-leetcode-/)
- 解法一：一次遍历
    + 只需要遍历一次数组，**用一个变量记录遍历过数中的最小值**，然后每次计算当前值和这个最小值之间的差值最为利润，然后每次选较大的利润来更新。当遍历完成后当前利润即为所求，代码如下：
    + 我自己些的
    ```C++
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            if(prices.size() <= 1)
                return 0;
            int res = 0;
            int base = prices[0];
            for(int i = 1; i < prices.size(); ++i)
            {
                if(prices[i] - base > res)
                {
                    res = prices[i] - base;
                }
                base = min(base, prices[i]);
            }
            return res;
        }
    };
    ```
    + 优化的写法
    ```C++
    class Solution {
    public:
        int maxProfit(vector<int>& prices) {
            int res = 0;
            int base = INT_MAX;
            for(int i = 0; i < prices.size(); ++i)
            {
                base = min(base, prices[i]);
                res = max(res, prices[i] - base);
            }
            return res;
        }
    };
    ```