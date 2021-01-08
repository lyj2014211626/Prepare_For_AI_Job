- [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4601208.html)
- [官方解法](https://leetcode-cn.com/problems/basic-calculator-ii/solution/chai-jie-fu-za-wen-ti-shi-xian-yi-ge-wan-zheng-ji-/)
- 把字符串分解成符号和数字的组合。
- 只需要一个保存数字的栈即可，不需要增加一个运算符的栈。
    ```C++
    class Solution {
    public:
        int calculate(string s) {
            stack<int> nums;
            char op = '+';
            long long int num = 0;
            int n = s.size();
            for(int i = 0; i < n; ++i)
            {
                if(s[i] >= '0')
                {
                    num = 10 * num + s[i] - '0';
                }
                if((s[i] < '0' && s[i] != ' ') || i == n - 1)
                {
                    if(op == '+')
                        nums.push(num);
                    else if(op == '-')
                        nums.push(-num);
                    else if(op == '*')
                    {
                        int t = nums.top() * num;
                        nums.pop();
                        nums.push(t);
                    }
                    else if(op == '/')
                    {
                        int t = nums.top() / num;
                        nums.pop();
                        nums.push(t);
                    }
                    op = s[i];
                    num = 0;
                }
            }
            long long int res = 0;
            while(!nums.empty())
            {
                res += nums.top();
                nums.pop();
            }
            return res;

        }
    };
    ````