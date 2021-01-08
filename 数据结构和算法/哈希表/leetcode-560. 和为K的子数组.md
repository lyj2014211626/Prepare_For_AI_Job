- [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)
- [参考解法](https://www.cnblogs.com/grandyang/p/6810361.html)
- [官方解法](https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/he-wei-kde-zi-shu-zu-by-leetcode-solution/)
- 哈希表
    + 论坛上大家比较推崇的其实是这种解法，用一个哈希表来建立连续子数组之和跟其出现次数之间的映射，初始化要加入 {0,1} 这对映射，这是为啥呢，因为我们的解题思路是遍历数组中的数字，用 sum 来记录到当前位置的累加和，我们建立哈希表的目的是为了让我们可以快速的查找 sum-k 是否存在，即是否有连续子数组的和为 sum-k，如果存在的话，那么和为k的子数组一定也存在，这样当 sum 刚好为k的时候，那么数组从起始到当前位置的这段子数组的和就是k，满足题意，如果哈希表中事先没有 m[0] 项的话，这个符合题意的结果就无法累加到结果 res 中，这就是初始化的用途。上面讲解的内容顺带着也把 for 循环中的内容解释了，这里就不多阐述了，有疑问的童鞋请在评论区留言哈，参见代码如下：
    ```C++
    class Solution {
    public:
        int subarraySum(vector<int>& nums, int k) {
            unordered_map<int, int> mp{{0, 1}};
            int res = 0;
            int sum = 0;
            for(int i = 0; i < nums.size(); ++i)
            {
                sum += nums[i];
                res += mp[sum - k];
                ++mp[sum];
            }
            return res;
        }
    };
    ```