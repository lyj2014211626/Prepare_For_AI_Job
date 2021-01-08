- [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)
- [参考解法](https://www.cnblogs.com/grandyang/p/5951422.html)
- [官方解法](https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-xiang-jie-zhen-dui-ben-ti-de-yo/)
- 解法一：动态规划之背包问题
    + 转化成背包问题。二维动态数组
    ```C++
    class Solution {
    public:
        bool canPartition(vector<int>& nums) {
            int sum = 0;
            for(auto num : nums)
                sum += num;
            if(sum % 2 == 1)
                return false;
             sum /= 2;
            int n = nums.size();
            vector<vector<bool>> dp(n + 1, vector<bool>(sum + 1, false));
            for(int i = 0; i <= n; ++i)
                dp[i][0] = true;

            for(int i = 1; i <= n; ++i)
            {
                for(int j = 1; j <= sum; ++j)
                {
                    if(j >= nums[i - 1])
                    {
                        dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i - 1]];
                    }
                    else
                        dp[i][j] = dp[i - 1][j];
                }
            }
            return dp[n][sum];
        }
    };
    ```

- 优化
    ```C++
    class Solution {
    public:
        bool canPartition(vector<int>& nums) {
            int sum = 0;
            for(auto num : nums)
                sum += num;
            if(sum % 2 == 1)
                return false;
             sum /= 2;
            int n = nums.size();
            vector<bool> dp(sum + 1, false);
            dp[0] = true;
            for(int i = 1; i <= n; ++i)
            {
                for(int j = sum; j >= 0; --j)
                {
                    if(j >= nums[i - 1])
                    {
                        dp[j] = dp[j] || dp[j - nums[i - 1]];
                    }
                }
            }
            return dp[sum];
        }
    };
    ```