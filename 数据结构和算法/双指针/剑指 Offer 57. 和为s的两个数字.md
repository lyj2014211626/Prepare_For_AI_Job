- [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)
- 解法一：哈希表
    + 不是最优解。类似计算二数之和。因为有个是有序数组的条件。时间复杂度是O(N),空间复杂度是O(N)。
    ```C++
    class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            unordered_map<int, int> tab;
            for(int i = 0; i < nums.size(); ++i)
            {
                if(tab.find(target - nums[i]) != tab.end())
                    return vector<int> {nums[i], target - nums[i]};
                else tab[nums[i]] = nums[i];
            }
            return {};
        }
};
    ```

- 解法二：双指针法
    + 时间复杂度是O(N),空间复杂度是O(1).
    ```C++
    class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            int l = 0;
            int r = nums.size()- 1;
            while(l < r)
            {
                int sum = nums[l] + nums[r];
                if(sum == target)
                    return {nums[l], nums[r]};
                else if(sum > target)
                    --r;
                else
                    ++l;
            }
            return {};
        }
    };
    ```