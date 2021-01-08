- [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)
- [参考解法](https://www.cnblogs.com/grandyang/p/7404777.html)
- [官方解法](https://leetcode-cn.com/problems/palindromic-substrings/solution/hui-wen-zi-chuan-by-leetcode/)
- 中心扩散法
    + 就是以字符串中的每一个字符都当作回文串中间的位置，然后向两边扩散，每当成功匹配两个左右两个字符，结果 res 自增1，然后再比较下一对。注意回文字符串有奇数和偶数两种形式，如果是奇数长度，那么i位置就是中间那个字符的位置，所以左右两遍都从i开始遍历；如果是偶数长度的，那么i是最中间两个字符的左边那个，右边那个就是 i+1，这样就能 cover 所有的情况啦，而且都是不同的回文子字符串，参见代码如下：
    ```C++
    class Solution {
    public:
        int countSubstrings(string s) {
            int cnt = 0;
            for(int i = 0; i < s.size(); ++i)
            {
                help(s, i, i, cnt);
                help(s, i, i + 1, cnt);
            }
            return cnt;
        }
        
        void help(string& s, int i, int j, int& cnt)
        {
            while(i >= 0 && j < s.size() && s[i] == s[j])
            {
                ++cnt;
                --i;
                ++j;
            }
        }
    };
    ```

- 动态规划
    + 和判断回文串的逻辑类似。
    ```C++
    class Solution {
    public:
        int countSubstrings(string s) {
            int cnt = 0;
            int n = s.size();
            vector<vector<bool>>dp(n, vector<bool>(n, 0));
            
            for(int i = n - 1; i >= 0; --i)
            {
                for(int j = i; j < n; ++j)
                {
                    dp[i][j] = (s[i] == s[j]) && (j - i < 2 || dp[i + 1][j - 1]);
                    if(dp[i][j])
                        ++cnt;
                }
            }
            return cnt;
        }
        
    };
    ```

- 自己的写法
    ```C++
    class Solution {
    public:
        int countSubstrings(string s) {
            int cnt = 0;
            int n = s.size();
            vector<vector<bool>>dp(n, vector<bool>(n, 0));
            
            for(int i = 0; i < s.size(); ++i)
            {
                for(int j = 0; j <= i; ++j)
                {
                    dp[j][i] = (s[i] == s[j]) && (i - j < 2 || dp[j + 1][i - 1]);
                    if(dp[j][i])
                        ++cnt;
                }
            }
            return cnt;
        }
        
    };
    ```
