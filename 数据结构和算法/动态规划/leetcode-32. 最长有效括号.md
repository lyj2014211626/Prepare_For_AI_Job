- [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4424731.html)
- [管方解法](https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode/)
- 解法一：栈
    + 这道求最长有效括号比之前那道 [Valid Parentheses](https://www.cnblogs.com/grandyang/p/4424587.html) 难度要大一些，这里还是借助栈来求解，**需要定义个 start 变量来记录合法括号串的起始位置**，遍历字符串，**如果遇到左括号，则将当前下标压入栈，如果遇到右括号，如果当前栈为空，则将下一个坐标位置记录到 start，如果栈不为空，则将栈顶元素取出，此时若栈为空，则更新结果和 i - start + 1 中的较大值，否则更新结果和 i - st.top() 中的较大值**，参见代码如下：
    ```C++
    class Solution {
    public:
        int longestValidParentheses(string s) {
            int res = 0;
            int n = s.size();
            stack<int> st;
            int start = 0;
            for(int i = 0; i < n; ++i)
            {
                if(s[i] == '(')
                    st.push(i);
                else if(s[i] == ')')
                {
                    if(st.empty())
                        start = i + 1;
                    else
                    {
                        st.pop();
                        res = st.empty() ? max(res, i - start + 1) : max(res, i - st.top());
                    }
                }
            }
            return res;
        }
    };
    ```

- 解法二：动态规划
    + 还有一种利用动态规划 Dynamic Programming 的解法。这里使用一个一维 dp 数组，其中 dp[i] 表示以 s[i-1] 结尾的最长有效括号长度（注意这里没有对应 s[i]，是为了避免取 dp[i-1] 时越界从而让 dp 数组的长度加了1），s[i-1] 此时必须是有效括号的一部分，那么只要 dp[i] 为正数的话，说明 s[i-1] 一定是右括号，因为有效括号必须是闭合的。当括号有重合时，比如 "(())"，会出现多个右括号相连，此时更新最外边的右括号的 dp[i] 时是需要前一个右括号的值 dp[i-1]，因为假如 dp[i-1] 为正数，说明此位置往前 dp[i-1] 个字符组成的子串都是合法的子串，需要再看前面一个位置，假如是左括号，说明在 dp[i-1] 的基础上又增加了一个合法的括号，所以长度加上2。但此时还可能出现的情况是，前面的左括号前面还有合法括号，比如 "()(())"，此时更新最后面的右括号的时候，知道第二个右括号的 dp 值是2，那么最后一个右括号的 dp 值不仅是第二个括号的 dp 值再加2，还可以连到第一个右括号的 dp 值，整个最长的有效括号长度是6。所以在更新当前右括号的 dp 值时，首先要计算出第一个右括号的位置，通过 i-3-dp[i-1] 来获得，由于这里定义的 dp[i] 对应的是字符 s[i-1]，所以需要再加1，变成 j = i-2-dp[i-1]，这样若当前字符 s[i-1] 是左括号，或者j小于0（说明没有对应的左括号），或者 s[j] 是右括号，此时将 dp[i] 重置为0，否则就用 dp[i-1] + 2 + dp[j] 来更新 dp[i]。这里由于进行了 padding，可能对应关系会比较晕，大家可以自行带个例子一步一步执行，应该是不难理解的，参见代码如下：
    ```C++
    class Solution {
    public:
        int longestValidParentheses(string s) {
            int res = 0;
            int n = s.size();
            vector<int> dp(n + 1, 0);
            for(int i = 1; i <= n; ++i)
            {
                int j = i - 2 - dp[i - 1];
                if(s[i - 1] == '(' || j < 0 || s[j] == ')')
                    dp[i] = 0;
                else
                {
                    dp[i] = dp[i - 1] + 2 + dp[j];
                    res = max(res, dp[i]);
                }
            }
            return res;
        }
    };
    ```

- 解法三：不需要额外的空间
    + 此题还有一种不用额外空间的解法，使用了两个变量 left 和 right，分别用来记录到当前位置时左括号和右括号的出现次数，当遇到左括号时，left 自增1，右括号时 right 自增1。对于最长有效的括号的子串，一定是左括号等于右括号的情况，此时就可以更新结果 res 了，一旦右括号数量超过左括号数量了，说明当前位置不能组成合法括号子串，left 和 right 重置为0。但是对于这种情况 "(()" 时，在遍历结束时左右子括号数都不相等，此时没法更新结果 res，但其实正确答案是2，怎么处理这种情况呢？答案是再反向遍历一遍，采取类似的机制，稍有不同的是此时若 left 大于 right 了，则重置0，这样就可以 cover 所有的情况了，参见代码如下：
    ```C++
    class Solution {
    public:
        int longestValidParentheses(string s) {
            int res = 0;
            int n = s.size();
            int left = 0, right = 0;
            for(int i = 0; i < n; ++i)
            {
                if(s[i] == '(')
                    ++left;
                else
                    ++right;
                if(left == right)
                    res = max(res, left + right);
                else if(right > left)
                {
                    left = 0;
                    right = 0;
                }
            }
            left = right = 0;
            for(int i = n - 1; i >= 0; --i)
            {
                if(s[i] == '(')
                    ++left;
                else
                    ++right;
                if(left == right)
                    res = max(res, left + right);
                else if(left > right)
                {
                    left = right = 0;
                }
            }
            return res;
        }
    };
    ```
