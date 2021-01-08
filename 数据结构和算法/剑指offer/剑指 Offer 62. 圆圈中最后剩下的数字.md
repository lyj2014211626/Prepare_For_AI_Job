- [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
- [参考解法](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-by-lee/)
- [参考解法](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/javajie-jue-yue-se-fu-huan-wen-ti-gao-su-ni-wei-sh/)
    + 反推最后出现的数字在之前各个轮次出现的位置。
    ```C++
    class Solution {
    public:
        int lastRemaining(int n, int m) {
            int res = 0;
            for(int i = 2; i <= n; ++i)
            {
                res = (res + m) % i;
            }
            return res;
        }
    };
    ```