- [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)
- [参考博客](https://github.com/grandyang/leetcode/issues/16)
- [官方解答](https://leetcode-cn.com/problems/3sum-closest/solution/hua-jie-suan-fa-16-zui-jie-jin-de-san-shu-zhi-he-b/)
- 解法：双指针+排序
    + 这道题让我们求最接近给定值的三数之和，是在之前那道 3Sum 的基础上又增加了些许难度，那么这道题让返回这个最接近于给定值的值，即要保证当前三数和跟给定值之间的差的绝对值最小，所以需要定义一个变量 diff 用来记录差的绝对值，然后还是要先将数组排个序，然后开始遍历数组，思路跟那道三数之和很相似，都是先确定一个数，然后用两个指针 left 和 right 来滑动寻找另外两个数，每确定两个数，求出此三数之和，然后算和给定值的差的绝对值存在 newDiff 中，然后和 diff 比较并更新 diff 和结果 closest 即可，代码如下：
    + C++版本
    ```
    class Solution {
    public:
        int threeSumClosest(vector<int>& nums, int target) {
            sort(nums.begin(), nums.end());
            int sum_num = nums[0] + nums[1] + nums[2];//初始化
            int diff = abs(target - sum_num);
            for(int i = 0; i < nums.size() - 2; ++i)
            {
                int l = i + 1;//二个指针从二端向中间搜索
                int r = nums.size() -1;
                while(l < r)
                {
                    int sum_new = nums[i] + nums[l] + nums[r];
                    int diff_new = abs(target - sum_new);
                    if(diff > diff_new)
                    {
                        diff = diff_new;
                        sum_num = sum_new;
                    }
                    if(sum_new < target)
                        ++l;
                    else
                        --r;
                }
            }
            return sum_num;
        }
    };
    ```
    + C++版本再优化
    + 我们还可以稍稍进行一下优化，每次判断一下，当 nums[i]*3 > target 的时候，就可以直接比较 closest 和 nums[i] + nums[i+1] + nums[i+2] 的值，返回较小的那个，因为数组已经排过序了，后面的数字只会越来越大，就不必再往后比较了，参见代码如下：
    ```
    class Solution {
    public:
        int threeSumClosest(vector<int>& nums, int target) {
            int closest = nums[0] + nums[1] + nums[2];
            int diff = abs(closest - target);
            sort(nums.begin(), nums.end());
            for (int i = 0; i < nums.size() - 2; ++i) {
                if (nums[i] * 3 > target) return min(closest, nums[i] + nums[i + 1] + nums[i + 2]);
                int left = i + 1, right = nums.size() - 1;
                while (left < right) {
                    int sum = nums[i] + nums[left] + nums[right];
                    int newDiff = abs(sum - target);
                    if (diff > newDiff) {
                        diff = newDiff;
                        closest = sum;
                    }
                    if (sum < target) ++left;
                    else --right;
                }
            }
            return closest;
        }
    };
    ```