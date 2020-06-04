- [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4461713.html)
- [官方解法](https://leetcode-cn.com/problems/regular-expression-matching/solution/zheng-ze-biao-da-shi-pi-pei-by-leetcode/)
- 解法一：递归
    + 每次从字符串里拿出一个字符和字符规律中字符去匹配，。
    + 先来**分析一个字符**：如果当前字符规律中的字符ch是'.'，那么它可以匹配任何字符。如果字符规律中的字符ch不是'.',而且字符串中的字符也是ch，那么他们相互匹配。当不匹配时，接着匹配下面的字符。
    + 当字符规律中**第二个字符**是'*',分二种情况考虑。一是第一个字符相等，则分为s不变，p后移二个字符和s后移一个字符和p不变。二是第一个字符不相等，则直接s不变，p后移二位。
    + 当字符规律中**第二个字符**不是'*',则直接匹配第一个字符是否相等，返回结果即可。
    ```C++
    class Solution {
    public:
        bool isMatch(string s, string p) {
            if(p.empty())
                return s.empty();
            if(p.size() > 1 && p[1] == '*')//第二个字符是'*'
            {
                if(s[0] == p[0] || (p[0] == '.' && !s.empty())) //第一个字符相等时
                {
                    return isMatch(s.substr(1), p)//非0次字符
                            || isMatch(s, p.substr(2));//0次字符
                }
                else
                {
                    return isMatch(s, p.substr(2));//0次字符
                }
            }
            else if(s[0] == p[0] || (p[0] == '.' && !s.empty()))//比较第一个字符
                return isMatch(s.substr(1), p.substr(1));
            else return false;
        }
    };
    ```
    + 简化
    + 先来判断p是否为空，若为空则根据s的为空的情况返回结果。当p的第二个字符为*号时，由于*号前面的字符的个数可以任意，可以为0，那么我们先用递归来调用为0的情况，就是直接把这两个字符去掉再比较，或者当s不为空，且第一个字符和p的第一个字符相同时，再对去掉首字符的s和p调用递归，注意p不能去掉首字符，因为*号前面的字符可以有无限个；如果第二个字符不为*号，那么就老老实实的比较第一个字符，然后对后面的字符串调用递归，参见代码如下：
    ```C++
    class Solution {
    public:
        bool isMatch(string s, string p) {
            if(p.empty())
                return s.empty();
            if(p.size() > 1 && p[1] == '*')
            {
                return isMatch(s, p.substr(2)) || ((s[0] == p[0] || p[0] == '.') && !s.empty() && isMatch(s.substr(1), p));
            }
            else
            {
                return (!s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1)));
            }
        }
    };
    ```

- 解法二：动态规划
    + 我们也可以用 DP 来解，定义一个二维的 DP 数组，其中 dp[i][j] 表示 s[0,i) 和 p[0,j) 是否 match，然后有下面三种情况([下面部分摘自这个帖子](https://leetcode.com/problems/regular-expression-matching/discuss/5684/c-on-space-dp))：
```
1.  P[i][j] = P[i - 1][j - 1], if p[j - 1] != '*' && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
2.  P[i][j] = P[i][j - 2], if p[j - 1] == '*' and the pattern repeats for 0 times;
3.  P[i][j] = P[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'), if p[j - 1] == '*' and the pattern repeats for at least 1 times.
```
    
    ```C++
    class Solution {
    public:
        bool isMatch(string s, string p) {
            int m = s.size();
            int n = p.size();
            vector<vector<int> >dp(m + 1, vector<int>(n + 1, 0));
            dp[0][0] = true;
            for(int i = 0; i <= m; ++i)
            {
                for(int j = 1; j <= n; ++j)
                {
                    if(j > 1 && p[j - 1] == '*')
                    {
                        dp[i][j] = dp[i][j - 2] || (i > 0 && (s[i - 1] == p[j - 2] || p[j - 2] == '.') && dp[i - 1][j]);
                    }
                    else
                    {
                        dp[i][j] = i > 0 && (s[i - 1] == p[j - 1] || p[j - 1] == '.') && dp[i - 1][j - 1];
                    }
                }
            }
            return dp[m][n];
        }
    };
    ```
    + 内存优化
    + 用二个一维数组
    ```C++
    class Solution {
    public:
        bool isMatch(string s, string p) {
            int m = s.size(), n = p.size();
            vector<bool> pre(n + 1, false), cur(n + 1, false);
            cur[0] = true;
            for (int i = 0; i <= m; i++) {
                for (int j = 1; j <= n; j++) {
                    if (p[j - 1] == '*') {
                        cur[j] = cur[j - 2] || (i > 0 && pre[j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'));
                    } else {
                        cur[j] = i > 0 && pre[j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
                    }
                }
                fill(pre.begin(), pre.end(), false);
                swap(pre, cur);
            }
            return pre[n];
        }
    };
    ```
    + 内存优化
    + 用一个一维数组
    ```C++
    class Solution {
    public:
        bool isMatch(string s, string p) {
            int m = s.size(), n = p.size();
            vector<bool> cur(n + 1, false);
            for (int i = 0; i <= m; i++) {
                bool pre = cur[0];
                cur[0] = !i;
                for (int j = 1; j <= n; j++) {
                    bool temp = cur[j];
                    if (p[j - 1] == '*') {
                        cur[j] = cur[j - 2] || (i && cur[j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'));
                    } else {
                        cur[j] = i && pre && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
                    }
                    pre = temp;
                }
            }
            return cur[n];
        }
    };
    ```