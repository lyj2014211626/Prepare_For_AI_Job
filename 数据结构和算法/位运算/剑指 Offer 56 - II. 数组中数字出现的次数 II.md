- [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)
- [官方解法](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/)
- 解法一：位运算
    ```C++
    class Solution {
    public:
        int singleNumber(vector<int>& nums) {
            vector<int> vec(32,0);
            for(int i = 0; i < nums.size(); ++i)
            {
                long long mask = 1;
                for(int j = 31; j >= 0; --j)
                {
                    if((nums[i] & mask))
                        vec[j] += 1;
                    mask <<= 1;
                }
            }
            int res = 0;
            for(int  i = 0; i < 32; ++i)
            {
                res <<= 1;
                res += vec[i] % 3;
            }
            return res;
        }
    };
    ```