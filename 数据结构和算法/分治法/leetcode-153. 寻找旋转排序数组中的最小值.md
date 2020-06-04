- [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4032934.html)
- [官方解法](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/er-fen-cha-zhao-wei-shi-yao-zuo-you-bu-dui-cheng-z/)
- 解法一：变种的二分法
    + 这道寻找旋转有序数组的最小值肯定不能通过直接遍历整个数组来寻找，这个方法过于简单粗暴，这样的话，旋不旋转就没有意义。应该考虑将时间复杂度从简单粗暴的 O(n) 缩小到 O(lgn)，这时候二分查找法就浮现在脑海。这里的二分法属于博主之前的总结帖 [LeetCode Binary Search Summary](https://www.cnblogs.com/grandyang/p/6854825.html) 二分搜索法小结 中的第五类，也是比较难的那一类，**没有固定的 target 值比较，而是要跟数组中某个特定位置上的数字比较，决定接下来去哪一边继续搜索。**这里用中间的值 nums[mid] 和右边界值 nums[right] 进行比较，若**数组没有旋转或者旋转点在左半段的时候，中间值是一定小于右边界值的**，所以要去左半边继续搜索，反之则去右半段查找，最终返回 nums[right] 即可，参见代码如下：
    + 这里的判断条件很苛刻，注意条件的取值和位置
    ```C++
    class Solution {
    public:
        int findMin(vector<int>& nums) {
            int left = 0;
            int right = nums.size() - 1;//这里是 - 1 
            while(left < right)
            {
                int mid = left + (right - left) / 2;    //这里默认下取整
                if(nums[mid] > nums[right]) //当大于的时候 该位置的元素肯定是不对的 所以索引加1
                    left = mid + 1;
                else
                    right = mid; //因为中值 < 右值，中值也可能是最小值，右边界只能取到mid处
            }
            return nums[left];
        }
    };
    ```

- 解法二：分治
    + fgf
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
            if(nums[left] <= nums[right])   // 递归结束的判断 返回
                return nums[left];
            int mid = left + (right - left) / 2;
            return min(cursion(nums, left, mid), cursion(nums, mid + 1, right));    //二叉树回溯 返回二个子节点的最小的值
        }
    };
    ```