- [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)
- [参考链接](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/solution/mian-shi-ti-46-ba-shu-zi-fan-yi-cheng-zi-fu-chua-6/)
- 解法一：动态规划
    + 类似于青蛙跳台阶，但是有些许的改变，就是判断n-2的值是>=10 && <= 25
    ```C++
    class Solution {
    public:
        int translateNum(int num) {
            string str = to_string(num);
            int pre = 1;
            int cur = 1;
            int res = cur;
            for(int i = 1; i < str.size(); ++i)
            {
                int t = 10 * (str[i - 1] - '0') + str[i] - '0';
                if(t >= 10 && t <= 25)
                    res = pre + cur;
                else
                    res = cur;
                pre = cur;
                cur = res;
            }
            return res;
        }
    };
    ```