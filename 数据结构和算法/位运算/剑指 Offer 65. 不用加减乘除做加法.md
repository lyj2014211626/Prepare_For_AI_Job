- [剑指 Offer 65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)
- 位运算
    + c++不支持负值左移，需要强制转换为无符号数
    ```C++
    class Solution {
    public:
        int add(int a, int b) {
            while(b != 0)
            {
                int t = a ^ b;
                b = (unsigned int)(a & b)<<1;
                a = t;
            }
            return a;
        }
    };
    ```