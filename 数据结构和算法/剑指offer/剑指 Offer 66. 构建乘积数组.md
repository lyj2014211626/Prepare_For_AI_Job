- [剑指 Offer 66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)
- [参考解法](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/solution/mian-shi-ti-66-gou-jian-cheng-ji-shu-zu-biao-ge-fe/)
- 上下三角行法
    ```C++
    class Solution {
    public:
        vector<int> constructArr(vector<int>& a) {
            if(a.empty())
                return {};
            int n = a.size();
            vector<int> vec(n, 1);
            int t = 1;
            for(int i = 1; i < n; ++i)
            {
                vec[i] = vec[i - 1] * a[i - 1];
            }
            for(int i = n - 2; i >= 0; --i)
            {
                t = t * a[i + 1];
                vec[i] *= t;
            }
            return vec;
        }
    };
    ```