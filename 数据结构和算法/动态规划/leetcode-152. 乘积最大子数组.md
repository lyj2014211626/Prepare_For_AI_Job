- [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)
- [参考博客](https://github.com/grandyang/leetcode/issues/152)
- 解法一：动态规划
    + 其实这道题最直接的方法就是用 DP 来做，而且要用两个 dp 数组，其中 f[i] 表示子数组 [0, i] 范围内并且一定包含 nums[i] 数字的最大子数组乘积，g[i] 表示子数组 [0, i] 范围内并且一定包含 nums[i] 数字的最小子数组乘积，初始化时 f[0] 和 g[0] 都初始化为 nums[0]，其余都初始化为0。那么从数组的第二个数字开始遍历，那么此时的最大值和最小值只会在这三个数字之间产生，即 f[i-1]*nums[i]，g[i-1]*nums[i]，和 nums[i]。所以用三者中的最大值来更新 f[i]，用最小值来更新 g[i]，然后用 f[i] 来更新结果 res 即可，由于最终的结果不一定会包括 nums[n-1] 这个数字，所以 f[n-1] 不一定是最终解，不断更新的结果 res 才是，参见代码如下：
    + 这里需要注意的是因为数有正负的关系，这里就需要二个数组，分布记录当前最大值和最小值。最大值和最小值很有可能交替产生。
    ```C++
    class Solution {
    public:
        int maxProduct(vector<int>& nums) {
            int res = nums[0];
            int n = nums.size();
            vector<int> f(n, 0);
            vector<int> g(n, 0);
            f[0] = nums[0];
            g[0] = nums[0];
            for(int i = 1; i < n; ++i)
            {
                f[i] = max(max(f[i - 1] * nums[i], g[i - 1] * nums[i]), nums[i]);
                g[i] = min(min(f[i - 1] * nums[i], g[i - 1] * nums[i]), nums[i]);
                res = max(res, f[i]);
            }
            return res;
        }
    };
    ```

- 解法二：优化
    + 这里保存的最大最小的数组可以用一个数保存
    ```C++
    class Solution {
    public:
        int maxProduct(vector<int>& nums) {
            int res = nums[0];
            int n = nums.size();
            int mn = nums[0];
            int mx = nums[0];
            for(int i = 1; i < n; ++i)
            {
                int tmx = mx;//这里要保存临时变量
                int tmn = mn;//
                mx = max(max(tmx * nums[i], tmn * nums[i]), nums[i]);
                mn = min(min(tmx * nums[i], tmn * nums[i]), nums[i]);
                res = max(res, mx);
            }
            return res;
        }
    };
    ```

