- [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)
- [参考解法](https://www.cnblogs.com/grandyang/p/5849037.html)
- [官方解法](https://leetcode-cn.com/problems/decode-string/solution/decode-string-fu-zhu-zhan-fa-di-gui-fa-by-jyd/)
- 解法一：栈
    + 我们也可以用迭代的方法写出来，当然需要用 stack 来辅助运算，我们用两个 stack，一个用来保存个数，一个用来保存字符串，我们遍历输入字符串，如果遇到数字，我们更新计数变量 cnt；如果遇到左括号，我们把当前 cnt 压入数字栈中，把当前t压入字符串栈中；如果遇到右括号时，我们取出数字栈中顶元素，存入变量k，然后给字符串栈的顶元素循环加上k个t字符串，然后取出顶元素存入字符串t中；如果遇到字母，我们直接加入字符串t中即可，参见代码如下：
    ```C++
    class Solution {
    public:
        string decodeString(string s) {
            stack<int> s_int;
            stack<string> s_str;
            int cnt = 0;
            string t = "";
            for(int i = 0; i < s.size(); ++i)
            {
                if(s[i] >= '0' && s[i] <= '9')
                {
                    cnt = cnt * 10 + s[i] - '0';
                }
                else if(s[i] == '[')
                {
                    s_int.push(cnt);
                    s_str.push(t);
                    cnt = 0;
                    t = "";
                }
                else if(s[i] == ']')
                {
                    for(int j = 0; j < s_int.top(); ++j)
                        s_str.top() += t;
                    s_int.pop();
                    t = s_str.top();
                    s_str.pop();
                }
                else
                    t += s[i];
            }
            return s_str.empty() ? t : s_str.top();
        }
    };
    ```

- 递归
    + 这道题让我们把一个按一定规则编码后的字符串解码成其原来的模样，编码的方法很简单，就是把重复的字符串放在一个括号里，把重复的次数放在括号的前面，注意括号里面有可能会嵌套括号，这题可以用递归和迭代两种方法来解，我们首先来看递归的解法，把一个括号中的所有内容看做一个整体，一次递归函数返回一对括号中解码后的字符串。给定的编码字符串实际上只有四种字符，数字，字母，左括号，和右括号。那么我们开始用一个变量i从0开始遍历到字符串的末尾，由于左括号都是跟在数字后面，所以首先遇到的字符只能是数字或者字母，如果是字母，直接存入结果中，如果是数字，循环读入所有的数字，并正确转换，那么下一位非数字的字符一定是左括号，指针右移跳过左括号，对之后的内容调用递归函数求解，注意我们循环的停止条件是遍历到末尾和遇到右括号，由于递归调用的函数返回了子括号里解码后的字符串，而我们之前把次数也已经求出来了，那么循环添加到结果中即可，参见代码如下
    ```C++
    class Solution {
    public:
        string decodeString(string s) {
            int i = 0;
            return decode(s, i);
        }
        string decode(string& s, int& i)//i必须是引用
        {
            string res = "";
            while(i < s.size() && s[i] != ']')
            {
                if(s[i] < '0' || s[i] > '9')
                {
                    res += s[i++];
                }
                else
                {
                    int cnt = 0;
                    while(s[i] >= '0' && s[i] <= '9')
                    {
                        cnt = 10 * cnt + s[i++] - '0';
                    }
                    ++i;
                    string t = decode(s, i);
                    ++i;
                    while(cnt-- > 0)
                    {
                        res += t;
                    }
                }
            }
            return res;
        }
    };
    ```