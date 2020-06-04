- [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
- [参考博客1](https://github.com/grandyang/leetcode/issues/22)
- [力扣上不错的解](https://leetcode-cn.com/problems/generate-parentheses/solution/hui-su-suan-fa-by-liweiwei1419/)
- 解法1：回溯+剪枝
    + 对于这种列出所有结果的题首先还是考虑用递归 Recursion 来解，由于字符串只有左括号和右括号两种字符，而且最终结果必定是左括号3个，右括号3个，所以这里定义两个变量 left 和 right 分别表示剩余左右括号的个数，如果在某次递归时，左括号的个数大于右括号的个数，说明此时生成的字符串中右括号的个数大于左括号的个数，即会出现 ')(' 这样的非法串，所以这种情况直接返回，不继续处理。如果 left 和 right 都为0，则说明此时生成的字符串已有3个左括号和3个右括号，且字符串合法，则存入结果中后返回。如果以上两种情况都不满足，若此时 left 大于0，则调用递归函数，注意参数的更新，若 right 大于0，则调用递归函数，同样要更新参数，参见代码如下：
    ```C++
    class Solution {
    public:
        vector<string> generateParenthesis(int n) {
            vector<string> res;
            cur(n, n, "", res);
            return res;
        }
        void cur(int left, int right, string s, vector<string> &res){
            if(left == 0 && right == 0){//满足条件 退出
                res.push_back(s);
                return;
            }
            if(left > right)//左括号的个数大于右括号的个数-剪枝 由于左括号是开始 哟永远小于右括号的个数
                return;
            if(left > 0)
                cur(left - 1, right, s + "(", res);
            if(right > 0)
                cur(left, right - 1, s + ")", res);
        }
    };
    ```