- [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4408638.html)
- [官方解法](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)
- 解法一：二分法
    + 类似于求左边界
    ```C++
    class Solution {
    public:
        int searchInsert(vector<int>& nums, int target) {
            int left = 0;
            int right = nums.size();
            while(left < right)
            {
                int mid = left + (right - left) / 2;
                if(nums[mid] == target)
                    return mid;
                else if(nums[mid] < target)
                    left = mid + 1;
                else
                    right = mid;
            }
            return left;
        }
    };
    ```