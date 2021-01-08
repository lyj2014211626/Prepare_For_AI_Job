- [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)
- [参考解法](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/solution/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-by-leetcode/)
- 解法一：异或
    ```C++
    class Solution {
    public:
        vector<int> singleNumbers(vector<int>& nums) {
            int res = 0;
            for(auto it:nums)
                res ^= it;
            int div = 1;
            while((div & res) == 0)//计算第一个是1的位
                div <<= 1;
            int a = 0, b = 0;
            for(auto it : nums)
                if(div & it)//0和非0作为结果，不是0 / 1二种结果
                    a ^= it;
                else
                    b ^= it;
            return vector<int>{a,b};
        }
    };
    ```