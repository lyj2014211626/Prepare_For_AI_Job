- [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
- 解法一：二分法
    + 左边界，找到大于索引的第一个元素位置即可。
    ```C++
    class Solution {
    public:
        int missingNumber(vector<int>& nums) {
            int l = 0;
            int r = nums.size();
            while(l < r)
            {
                int mid = l + (r - l) / 2;
                if(nums[mid] == mid)
                    l = mid + 1;
                else if(mid != nums[mid])
                    r = mid;
            }
            return l;
        }
    };
    ```