- [263. 丑数](https://leetcode-cn.com/problems/ugly-number/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4741934.html)
- 解法一：
    + 判断是否能整除因子，然后进行商处理即可。
    ```C++
    class Solution {
    public:
        bool isUgly(int num) {
            bool flag = num;    //是0的话 直接为负
            while(flag && num != 1) //整除后为1  跳出即可
            {
                if(num % 2 == 0)
                {
                    num /= 2;
                    flag = true;
                }
                else if(num % 3 == 0)
                {
                    num /= 3;
                    flag = true;
                }
                else if(num % 5 == 0)
                {
                    num /= 5;
                    flag = true;
                }
                else
                    flag = false;
            }
            return flag;
        }
    };
    ```
- 解法二：
    + 这道题让我们检测一个数是否为丑陋数，所谓丑陋数就是其质数因子只能是 2，3，5。那么最直接的办法就是不停的除以这些质数，如果剩余的数字是1的话就是丑陋数了，参见代码如下：
    ```C++
    class Solution {
    public:
        bool isUgly(int num) {
            if(num <= 0)
                return false;
            while(num >= 2)
            {
                if(num % 2 == 0)
                    num /= 2;
                else if(num % 3 == 0)
                    num /= 3;
                else if(num % 5 == 0)
                    num /= 5;
                else
                    return false;
            }
            return true;
        }
    };
    ```

- 解法三：
    + 我们也可以换一种写法，分别不停的除以 2，3，5，并且看最后剩下来的数字是否为1即可，参见代码如下：
    ```C++
    class Solution {
    public:
        bool isUgly(int num) {
            if(num <= 0)
                return false;
            while(num % 2 == 0)
                num /= 2;
            while(num % 3 == 0)
                num /= 3;
            while(num % 5 == 0)
                num /= 5;
            return num == 1;
        }
    };
    ```