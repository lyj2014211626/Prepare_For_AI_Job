- [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/comments/)
- [参考解法](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shi-yao-shi-hua-dong-chuang-kou-yi-ji-ru-he-yong-h/)
- 解法：活动窗口或者指针方法。
    ```C++
    class Solution {
    public:
        vector<vector<int>> findContinuousSequence(int target) {
            int l = 1;
            int r = 1;
            int sum = 0;
            vector<vector<int>>res;
            while( l <= target / 2)//左边界一定在目标数字的中间以前
            {
                if(sum < target)
                {
                    sum += r;//先求之前的和
                    ++r;
                }
                else if(sum > target)
                {
                    sum -= l;
                    ++l;
                }
                else
                {
                    vector<int>vec;
                    for(int k = l; k < r; ++k)
                        vec.push_back(k);
                    res.push_back(vec);
                    sum -= l;
                    ++l;
                }
            }
            return res;
        }
    };
    ```