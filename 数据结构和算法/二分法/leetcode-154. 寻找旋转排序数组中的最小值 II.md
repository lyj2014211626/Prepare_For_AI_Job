- [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4040438.html)
- [官方解法](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/154-find-minimum-in-rotated-sorted-array-ii-by-jyd/)
- 解法一：二分法
    + 这道寻找旋转有序重复数组的最小值是之前那道 Find Minimum in Rotated Sorted Array 的拓展，当数组中存在大量的重复数字时，就会破坏二分查找法的机制，将无法取得 O(lgn) 的时间复杂度，又将会回到简单粗暴的 O(n)，比如这两种情况：{2, 2, 2, 2, 2, 2, 2, 2, 0, 1, 1, 2} 和 {2, 2, 2, 0, 2, 2, 2, 2, 2, 2, 2, 2}，可以发现，当第一个数字和最后一个数字，还有**中间那个数字全部相等的时候，二分查找法就崩溃了，因为它无法判断到底该去左半边还是右半边**。这种情况下，**将右指针左移一位（或者将左指针右移一位）**，略过一个相同数字，这对结果不会产生影响，因为只是去掉了一个相同的，然后对剩余的部分继续用二分查找法，在最坏的情况下，比如数组所有元素都相同，时间复杂度会升到 O(n)，参见代码如下：
    ```C++
    class Solution {
    public:
        int findMin(vector<int>& nums) {
            int left = 0;
            int right = nums.size() - 1;
            while(left < right)
            {
                int mid = left + (right - left) / 2;
                if(nums[mid] > nums[right])
                    left = mid + 1;
                else if(nums[mid] < nums[right])
                    right = mid;
                else
                    --right;//关键
            }
            return nums[left];
        }

    };
    ```

- 解法二：分治法
    + 跟之前那道 Find Minimum in Rotated Sorted Array 一样，还是可以用分治法 Divide and Conquer 来解，还是由热心网友 howard144 提供，不过写法跟之前那道略有不同，只有在 nums[start] < nums[end] 的时候，才能返回 nums[start]，等于的时候不能返回，比如 [3, 1, 3] 这个数组，或者当 start 等于 end 成立的时候，也可以直接返回 nums[start]，后面的操作跟之前那道题相同，每次将区间 [start, end] 从中间 mid 位置分为两段，分别调用递归函数，并比较返回值，每次取返回值较小的那个即可，参见代码如下：
    ```C++
    class Solution {
    public:
        int findMin(vector<int>& nums) {
            int left = 0;
            int right = nums.size() - 1;
            return cursion(nums, left, right);
        }
        int cursion(vector<int>& nums, int left, int right)
        {
            if(left == right)   //遍历到子数组只剩下一个元素的时候  返回
                return nums[left];
            if(nums[left] < nums[right])//是有序的子数组 直接返回最小值
                return nums[left];
            int mid = left + (right - left) / 2;
            return min(cursion(nums, left, mid), cursion(nums, mid + 1, right));
        }

    };
    ```