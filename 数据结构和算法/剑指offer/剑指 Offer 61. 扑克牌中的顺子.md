- [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)
- [官方解法](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/solution/mian-shi-ti-61-bu-ke-pai-zhong-de-shun-zi-ji-he-se/)
- 解法一
    ```C++
    class Solution {
    public:
        bool isStraight(vector<int>& nums) {
            sort(nums.begin(),nums.end());
            int zero_cnt = 0;
            int max_num;
            int min_num;
            for(int i = 0; i < 5; ++i)
            {
                if(nums[i] == 0)
                    ++zero_cnt;
                if(i > 0 && nums[i] != 0 && nums[i] == nums[i - 1])
                    return false;
            }
            max_num = nums[4];
            min_num = nums[zero_cnt];
            if(max_num - min_num <= 4)
                return true;
            else
                return false;
        }
    };
    ```