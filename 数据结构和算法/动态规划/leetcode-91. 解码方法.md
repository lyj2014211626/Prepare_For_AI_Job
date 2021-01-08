- [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4313384.html)
- [官方解法](https://leetcode-cn.com/problems/decode-ways/solution/c-wo-ren-wei-hen-jian-dan-zhi-guan-de-jie-fa-by-pr/)
- 解法一：动态规划
    + 对边界条件的判断。
    ```C++
    class Solution {
    public:
        int numDecodings(string s) {
            if(s.empty() || s[0] == '0')
                return 0;
            int pre = 1;
            int cur = 1;
            int res = 0;
            if(s[0] != '0')
                res = 1;
            int t = 0;
            for(int i = 1; i < s.size(); ++i)
            {
                t = 10 * (s[i - 1] - '0') + s[i] - '0';
                if(t == 10 || t == 20)
                    res = pre;
                else if(t >= 11 && t <= 26)
                {
                    res = pre + cur;
                }
                else if(t == 0 || (s[i - 1] >= '3' && s[i] == '0'))
                    return 0;
                else
                    res = cur;
                pre = cur;
                cur = res;
            }
            return res;
        }
    };
    ```

- 进一步整理逻辑
    ```C++
    class Solution {
    public:
        int numDecodings(string s) {
            if(s.empty() || s[0] == '0')
                return 0;
            int pre = 1;
            int cur = 1;
            int res = 0;
            if(s[0] != '0')
                res = 1;
            int t = 0;
            for(int i = 1; i < s.size(); ++i)
            {
                if(s[i] == '0')
                {
                    if(s[i - 1] == '1' || s[i - 1] == '2')
                        res = pre;
                    else
                        return 0;
                }
                else if(s[i - 1] == '1' || (s[i - 1] == '2' && s[i] >= '1' && s[i] <= '6'))
                {
                    res = pre + cur;
                }
                pre = cur;
                cur = res;
            }
            return res;
        }
    };
    ```