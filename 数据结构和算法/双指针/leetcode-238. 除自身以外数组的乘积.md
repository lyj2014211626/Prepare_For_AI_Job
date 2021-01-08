- [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4650187.html)
- [官方解法](https://leetcode-cn.com/problems/product-of-array-except-self/solution/chu-zi-shen-yi-wai-shu-zu-de-cheng-ji-by-leetcode-/)
- 左右乘积列表
    + 我们不必将所有数字的乘积除以给定索引处的数字得到相应的答案，而是利用索引左侧所有数字的乘积和右侧所有数字的乘积（即前缀与后缀）相乘得到答案。

    + 对于给定索引 ii，我们将使用它左边所有数字的乘积乘以右边所有数字的乘积。下面让我们更加具体的描述这个算法。
    ```C++
    class Solution {
    public:
        vector<int> productExceptSelf(vector<int>& nums) {
            if(nums.empty())
                return {};
            int n = nums.size();
            vector<int> output(n, 0);
            vector<int> L(n, 1);
            vector<int> R(n, 1);
            for(int i = 1; i < n; ++i)
            {
                L[i] = L[i - 1] * nums[i - 1];
            }
            for(int i = n - 2; i >= 0; --i)
            {
                R[i] = R[i + 1] * nums[i + 1];
            }
            for(int i = 0; i < n; ++i)
            {
                output[i] = L[i] * R[i];
            }
            return output;
        }
    };
    ```

- 空间优化
    ```C++
    class Solution {
    public:
        vector<int> productExceptSelf(vector<int>& nums) {
            if(nums.empty())
                return {};
            int n = nums.size();
            vector<int> output(n, 1);
            for(int i = 1; i < n; ++i)
            {
                output[i] = output[i - 1] * nums[i - 1];
            }
            int right = 1;
            for(int i = n - 1; i >= 0; --i)
            {
                output[i] *= right;
                right *= nums[i];
            }
            return output;
        }
    };
    ```