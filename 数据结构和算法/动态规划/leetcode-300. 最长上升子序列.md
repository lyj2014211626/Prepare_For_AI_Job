- [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
- [参考博客]()
- 解法一：动态规划
    + 首先来看一种动态规划 Dynamic Programming 的解法，这种解法的时间复杂度为 O(n2)，类似 brute force 的解法，维护一个一维 dp 数组，**其中 dp[i] 表示以 nums[i] 为结尾的最长递增子串的长度**，对于每一个 nums[i]，从第一个数再搜索到i，如果发现某个数小于 nums[i]，更新 dp[i]，**更新方法为 dp[i] = max(dp[i], dp[j] + 1)**，即比较当前 dp[i] 的值和那个小于 num[i] 的数的 dp 值加1的大小，就这样不断的更新 dp 数组，到最后 dp 数组中最大的值就是我们要返回的 LIS 的长度，参见代码如下：
    ```C++
    class Solution {
    public:
        int lengthOfLIS(vector<int>& nums) {
            vector<int> dp(nums.size(), 1);
            int res = 0;
            for(int i = 0; i < nums.size(); ++i)
            {
                for(int j = 0; j < i; ++j)
                {
                    if(nums[i] > nums[j])
                        dp[i] = max(dp[i], dp[j] + 1);
                }
                res = max(res, dp[i]);
            }
            return res;
        }
    };
    ```

- 解法二：二分 + DP
    + 思路是先建立一个空的 dp 数组，然后开始遍历原数组，对于每一个遍历到的数字，用二分查找法在 dp 数组找**第一个不小于它的数字**，如果这个数字不存在，那么直接在 dp 数组后面加上遍历到的数字，如果存在，则将这个数字更新为当前遍历到的数字，最后返回 dp 数组的长度即可，注意的是，跟上面的方法一样，特别注意的是 dp 数组的值可能不是一个真实的 LIS。参见代码如下：
    ```C++
    class Solution {
    public:
        int lengthOfLIS(vector<int>& nums) {
            if(nums.empty())
                return 0;
            vector<int> vec;
            for(int i = 0; i < nums.size(); ++i)
            {
                int left = 0;
                int right = vec.size();
                while(left < right)//二分查找左边界 即第一个大于目标元素的下标
                {
                    int mid = left + (right - left) / 2;
                    if(nums[i] > vec[mid])
                    {
                         left = mid + 1;
                    }
                    else
                        right = mid;
                }
                if(left >= vec.size())//比所有的都大 直接添加
                    vec.push_back(nums[i]);
                else
                    vec[left] = nums[i];//在范围之间  直接替换
            }
            return vec.size();
        }
    };
    ```