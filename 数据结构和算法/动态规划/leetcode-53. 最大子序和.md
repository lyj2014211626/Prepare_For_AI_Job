- [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
- [参考博客1](https://github.com/grandyang/leetcode/issues/53)
- [参考答案2](https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-by-leetcode/)
- 解法一：动态规划
    + 这道题让求最大子数组之和，并且要用两种方法来解，分别是 O(n) 的解法，还有用分治法 Divide and Conquer Approach，这个解法的时间复杂度是 O(nlgn)，那就先来看 O(n) 的解法，定义两个变量 res 和 curSum，其中 res 保存最终要返回的结果，即最大的子数组之和，curSum 初始值为0，每遍历一个数字 num，比较 **curSum + num** 和 **num** 中的较大值存入 curSum(这其实就是递归关系式)，然后再把 res 和 curSum 中的较大值存入 res，以此类推直到遍历完整个数组，可得到最大子数组的值存在 res 中，代码如下：
    ```
    class Solution {
    public:
        int maxSubArray(vector<int>& nums) {
            int res = INT_MIN, cur_sum = 0;//注意res 的初始化值
            for(int i = 0; i < nums.size(); ++i){
                cur_sum = max(nums[i], cur_sum + nums[i]);//其实就是该数字之前的和大于零和小于零的判断 相当于求解子问题
                res = max(res, cur_sum);//求子问题的最大值
            }
            return res;
        }
    };
    ```

- 解法二：分治法
    + 其实就是它的**最大子序和要么在左半边，要么在右半边，要么是穿过中间，对于左右边的序列，情况也是一样**，因此可以用递归处理。中间部分的则可以直接计算出来，时间复杂度应该是 O(nlogn)。代码如下：

    ```
    class Solution {
    public:
        int maxSubArray(vector<int>& nums) {
            if(nums.empty())
                return 0;
            return helper(nums, 0, nums.size() - 1);
        }
        int helper(vector<int>& nums, int left, int right){
            if(left >= right)//向上返回的边界条件
                return nums[left];
            int mid = left + ((right - left) >> 1);
            int lmax = helper(nums, left, mid - 1);//递归计算左边子序和
            int rmax = helper(nums, mid + 1, right);//递归计算右边子序和
            //计算中间的最大子序和，从右到左计算左边的最大子序和，从左到右计算右边的最大子序和，再相加
            int mmax = nums[mid];
            int t = mmax;
            for(int i = mid - 1; i >= left; --i){
                t = t + nums[i];//表示加到序列i左边的序列和
                mmax = max(mmax, t);
                //mmax = max(mmax, mmax + nums[i]);//错误的写法
            }
            t = mmax;
            for(int i = mid + 1; i <= right; ++i){
                t = t + nums[i];
                mmax = max(mmax, t);
                //mmax = max(mmax, mmax + nums[i]);//错误的写法
            }
            return max(mmax, max(lmax, rmax));//返回三个中间的最大值
        }
    };
    ```
