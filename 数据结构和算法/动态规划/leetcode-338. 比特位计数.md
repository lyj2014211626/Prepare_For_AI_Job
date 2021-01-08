- [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)
- [参考解法](https://www.cnblogs.com/grandyang/p/5294255.html)
- [官方解法](https://leetcode-cn.com/problems/counting-bits/solution/bi-te-wei-ji-shu-by-leetcode/)
- 解法一：偶数，是2的倍数，是将原数/2左移一位得到的结果。奇数，则是在奇数/2的下边界左移一位，加1得到的结果。
    + 状态转移方程
    + P(x)=P(x/2)+(xmod2)
    ```C++
    class Solution {
    public:
        vector<int> countBits(int num) {
            vector<int> nums(num + 1, 0);
            for(int i = 1; i <= num; ++i)
            {
                nums[i] = nums[i / 2] + i % 2;
            }
            return nums;
        }
    };
    ```

- 解法二
    + 巧妙的利用了 i&(i - 1)， 这个本来是用来判断一个数是否是2的指数的快捷方法，
    + P(x)=P(x&(x−1))+1;
    ```C++
    class Solution {
    public:
        vector<int> countBits(int num) {
            vector<int> nums(num + 1, 0);
            for(int i = 1; i <= num; ++i)
            {
                nums[i] = nums[i & (i - 1)] + 1;
            }
            return nums;
        }
    };
    ```