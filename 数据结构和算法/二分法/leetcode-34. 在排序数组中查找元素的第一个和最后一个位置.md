- [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4409379.html)
- [官方解法](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/solution/mian-shi-ti-53-i-zai-pai-xu-shu-zu-zhong-cha-zha-5/)
- 解法一：二分法
    + 用二分法求一个数字在数组中的左右边界即可。
    ```C++
    class Solution {
    public:
        vector<int> searchRange(vector<int>& nums, int target) {
            int l = 0;
            int r = nums.size();
            vector<int> res(2,-1);
            while(l < r)
            {
                int mid = l + (r - l) / 2;
                if(nums[mid] >= target)
                    r = mid;
                else
                    l = mid + 1;
            }
            if(l >= nums.size() || nums[l] != target)
                return res;
            res[0] = l;
            r = nums.size();
            while( l < r )
            {
                int mid = l + (r - l) / 2;
                if(nums[mid] <= target)
                    l = mid + 1;
                else
                    r = mid;
            }
            res[1] = l - 1;
            return res;
        }
    };
    ```

