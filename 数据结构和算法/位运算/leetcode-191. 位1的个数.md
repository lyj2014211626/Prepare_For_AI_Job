- [191. 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4325432.html)
- [官方解法](https://leetcode-cn.com/problems/number-of-1-bits/solution/wei-1de-ge-shu-by-leetcode/)
- 解法一：常规解法
    + 判断最后以为是否是1，然后右移一位继续判断最后一位是否是1即可。
    + 
    ```C++
    class Solution {
    public:
        int hammingWeight(uint32_t n) {
            int res = 0;
            while(n)    //  如果n是负数会导致死循环，如0x80000 -> 0xFFFFF
            {
                if(n % 2 == 1)
                    ++res;
                n = n >> 1;//比较于/2，更快
            }
            return res;
        }
    };
    ```
- 解法二：解法负数的死循环问题
    + 为了避免死循环，我们这里不对n进行移动，而是选择从1开始的正整数按位和操作数进行比较。
    ```C++
    class Solution {
    public:
        int hammingWeight(uint32_t n) {
            int res = 0;
            unsigned int flag = 1;
            while(flag)
            {
                if(n & flag)
                    ++res;
                flag = flag << 1;
            }
            return res;
        }
    };
    ```
- 解法三：只操作位上是1的个数
    + 一个数减去1，总是把最右边的1变成0.
    + **把一个整数减去1之后再和原来的整数做位与运算，得到的结果相当于把整数的二进制表示中最右边的1变成0。很多二进制的问题都可用这种思路解决。**
    ```C++
    class Solution {
    public:
        int hammingWeight(uint32_t n) {
            int res = 0;
            while(n)
            {
                ++res;
                n = (n - 1) & n;
            }
            return res;
        }
    };
    ```