- [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4518674.html)
- [官方解法](https://leetcode-cn.com/problems/house-robber-ii/solution/213-da-jia-jie-she-iidong-tai-gui-hua-jie-gou-hua-/)
- 解法一：动态规划
    + 其实就是把环拆成两个队列，一个是从0到n-1，另一个是从1到n，然后返回两个结果最大的。
    ```C++
    class Solution {
    public:
        int rob(vector<int>& nums) {
            int n = nums.size();
            vector<int> dp(n, 0);
            if(nums.empty())
                return 0;
            if(nums.size() == 1)
                return nums[0];
            if(nums.size() == 2)
                return max(nums[0], nums[1]);
            dp[0] = nums[0];
            dp[1] = max(nums[0], nums[1]);
            for(int i = 2; i < n - 1; ++i)
            {
                dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
            }
            vector<int> dp1(n, 0);
            dp1[0] = nums[0];
            dp1[1] = nums[1];
            dp1[2] = max(nums[1],nums[2]);
            for(int i = 3; i < n; ++i)
            {
                dp1[i] = max(dp1[i - 1], dp1[i - 2] + nums[i]);
            }
            return max(dp[n - 2], dp1[n - 1]);
        }
    };
    ```

- 优化
    - 二个变量的优化
    ```C++
    class Solution {
    public:
        int rob(vector<int>& nums) {
            int n = nums.size();
            vector<int> dp(n, 0);
            if(nums.empty())
                return 0;
            if(nums.size() == 1)
                return nums[0];
            if(nums.size() == 2)
                return max(nums[0], nums[1]);
            int pre = 0;
            int now = nums[0];
            int res1 = 0;
            int res2 = 0;
            for(int i = 1; i < n - 1; ++i)
            {
                res1 = max(now, pre + nums[i]);
                pre = now;
                now = res1;
            }
            pre = 0;
            now = nums[1];
            for(int i = 2; i < n; ++i)
            {
                res2 = max(now, pre + nums[i]);
                pre = now;
                now = res2;
            }
            return max(res1, res2);
        }
    };
    ```