- [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4424587.html)
- [官方题解](https://leetcode-cn.com/problems/valid-parentheses/solution/you-xiao-de-gua-hao-by-leetcode/)
- 解法一：栈
    + 这道题让我们验证输入的字符串是否为括号字符串，包括大括号，中括号和小括号。这里需要用一个栈，开始遍历输入字符串，如果当前字符为左半边括号时，则将其压入栈中，如果遇到右半边括号时，若此时栈为空，则直接返回 false，如不为空，则取出栈顶元素，若为对应的左半边括号，则继续循环，反之返回 false，代码如下：
    ```C++
    class Solution {
    public:
        bool isValid(string s) {
            stack<char> st;
            for(int i = 0; i < s.size(); ++i)
            {
                if(s[i] == '(' || s[i] == '{' || s[i] == '[')
                    st.push(s[i]);
                else
                {
                    if(st.empty())
                        return false;
                    if(s[i] == ')' && st.top() != '(')
                        return false;
                    if(s[i] == ']' && st.top() != '[')
                        return false;
                    if(s[i] == '}' && st.top() != '{')
                        return false;
                    st.pop();
                }
            }
            return st.empty();
        }
    };
    ```