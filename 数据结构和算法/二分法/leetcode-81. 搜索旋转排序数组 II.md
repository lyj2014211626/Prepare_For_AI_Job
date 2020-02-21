- [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
- [参考答案](https://github.com/grandyang/leetcode/issues/81)
- 解法一
    + 二次二分法。首先遍历数组找到顺序发生变化的位置，然后将该位置左右部分用二分法查找就行了。
    ```
    class Solution {
    public:
        bool search(vector<int>& nums, int target) {
            if (nums.empty())
                return false;
            int break_ind = 0;
            for(int i = 1; i < nums.size(); ++i){//找到顺序变化的位置
                if(nums[i] < nums[i - 1]){
                    break_ind = i;
                    break;
                }
            }
            int left = 0;//第一次用二分法
            int right = break_ind;
            while(left < right){
                int mid = left + ((right - left) >> 1);
                if(nums[mid] == target)
                    return true;
                if (nums[mid] < target)
                    left = mid + 1;
                else
                    right = mid;
            }

            left = break_ind;//这里注意，不是 break_ind + 1 总的区间是左闭右开
            right = nums.size();
            while(left < right){
                int mid = left + ((right - left) >> 1);
                if(nums[mid] == target)
                    return true;
                if (nums[mid] < target)
                    left = mid + 1;
                else
                    right = mid;
            }
            return false;
        }
    };
    ```

- 解法二
    + 这道是之前那道 Search in Rotated Sorted Array 的延伸，现在数组中允许出现重复数字，这个也会影响我们选择哪半边继续搜索，由于之前那道题不存在相同值，我们在比较中间值和最右值时就完全符合之前所说的规律： **如果中间的数小于最右边的数，则右半段是有序的，若中间数大于最右边数，则左半段是有序的** 。而如果可以有重复值，就会出现来面两种情况，[3 1 1] 和 [1 1 3 1]，对于这两种情况中间值等于最右值时，目标值3既可以在左边又可以在右边，那怎么办么，对于这种情况其实处理非常简单，只要把最右值向左一位即可继续循环，如果还相同则继续移，直到移到不同值为止，然后其他部分还采用 Search in Rotated Sorted Array 中的方法，可以得到代码如下：
    ```
    class Solution {
    public:
        bool search(vector<int>& nums, int target) {
            if (nums.empty())
                return false;
            int n = nums.size();
            int left = 0;
            int right = n - 1;
            while(left <= right){
                int mid = left + ((right - left) >> 1);
                if(nums[mid] == target)
                    return true;
                if(nums[mid] < nums[right]){//和右边界比较 右半段有序
                    if(nums[mid] < target && target <= nums[right])//右半段的右半段 注意等于号
                        left = mid + 1;
                    else
                        right = mid - 1;
                }
                else if(nums[mid] > nums[right]){//和右边界比较 左半段有序
                    if(nums[left] <= target && nums[mid] > target){//左半段的左半段 注意等于号
                        right = mid - 1;
                    }
                    else
                        left = mid + 1;
                }
                else
                    --right;
            }
            return false;
        }
    };
    ```