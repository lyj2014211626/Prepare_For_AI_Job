- [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4843654.html)
- [官方解法](https://leetcode-cn.com/problems/find-the-duplicate-number/solution/xun-zhao-zhong-fu-shu-by-leetcode-solution/)
- 解法一：二分法
    + 在区间 [1, n] 中搜索，首先求出中点 mid，然后遍历整个数组，统计所有小于等于 mid 的数的个数，如果个数小于等于 mid，则说明重复值在 [mid+1, n] 之间，反之，重复值应在 [1, mid-1] 之间，然后依次类推，直到搜索完成，此时的 low 就是我们要求的重复值，参见代码如下：
    ```C++
    class Solution {
    public:
        int findDuplicate(vector<int>& nums) {
            int left = 1;
            int right = nums.size();
            while(left < right)
            {
                int mid = left + (right - left) / 2;
                int cnt = 0;
                for(auto num : nums)
                    if(num <= mid)
                        ++cnt;
                if(cnt <= mid)
                    left = mid + 1;
                else
                    right = mid;
            }
            return left;
        }
    };
    ```

- 解法二：双指针
    + 可以这么理解，就是有一个数字重复，就存在二个重复下标指向通过一个数字，这个数字就对应环的入口。
    ```C++class Solution {
    public:
        int findDuplicate(vector<int>& nums) {
            int slow = 0;
            int fast = 0;
            int ind = 0;
            while(true)
            {
                slow = nums[slow];
                fast = nums[nums[fast]];
                if(slow == fast)
                    break;
            }
            while(true)
            {
                slow = nums[slow];
                ind = nums[ind];
                if(ind == slow)
                    break;
            }
            return slow;
        }
    };
    ```