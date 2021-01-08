- [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4383632.html)
- [官方解法](https://leetcode-cn.com/problems/house-robber/solution/hua-jie-suan-fa-198-da-jia-jie-she-by-guanpengchn/)
- 解法一：动态规划
    + 动态规划方程：dp[n] = MAX( dp[n-1], dp[n-2] + num )
    ```C++
    class Solution {
    public:
        int rob(vector<int>& nums) {
            if(nums.empty())
                return 0;
            if(nums.size() == 1)
                return nums[0];
            int n = nums.size();
            vector<int> dp(n, 0);
            dp[0] = nums[0];
            dp[1] = max(dp[0], nums[1]);
            for(int i = 2; i < n; ++i)
            {
                dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
            }
            return dp[n - 1];
        }
    };
    ```

- 优化
    + 用二个变量保存之前的记录即可。
    ```C++
    class Solution {
    public:
        int rob(vector<int>& nums) {
            if(nums.empty())
                return 0;
            if(nums.size() == 1)
                return nums[0];
            int n = nums.size();
            int pre = 0;
            int now = nums[0];
            int res = 0;
            for(int i = 1; i < n; ++i)
            {
                res = max(now, pre + nums[i]);
                pre = now;
                now = res;
            }
            return res;
        }
    };
    ```