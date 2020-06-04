- [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
- [参考代码](https://github.com/grandyang/leetcode/issues/17)
- 解法一：回溯
    + 这里可以用递归 Recursion 来解，需要建立一个字典，用来保存每个数字所代表的字符串，然后还需要一个变量 level（**这个是在递归中来回传递改变的变量**），记录当前生成的字符串的字符个数，实现套路和上述那些题十分类似。在递归函数中首先判断 level，如果跟 digits 中数字的个数相等了，将当前的组合加入结果 res 中，然后返回。我们通过 digits 中的数字到 dict 中取出字符串，然后遍历这个取出的字符串，将每个字符都加到当前的组合后面，并调用递归函数即可，参见代码如下：
    + C++版本 这是我自己写的
    ```C++
    class Solution {
    public:
        vector<string> letterCombinations(string digits) {
            vector<string> num_str(10, "");
            vector<string> res;
            if(digits.empty())//对边界条件的判断
                return res;
            string s;
            num_str[2] = "abc";//这里可以用vector<string> dict{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
            num_str[3] = "def";
            num_str[4] = "ghi";
            num_str[5] = "jkl";
            num_str[6] = "mno";
            num_str[7] = "pqrs";
            num_str[8] = "tuv";
            num_str[9] = "wxyz";
            cursion(0, digits, num_str, s, res);
            return res;
        }
        void cursion(int pos, string digits,vector<string>& num_str, string& path, vector<string>& res){
            if(pos >= digits.size())
            {
                res.push_back(path);
                return;
            }
                
            for(int i = 0; i < num_str[digits[pos] - '0'].size(); ++i){
                path += num_str[digits[pos] - '0'][i];
                cursion(pos + 1, digits, num_str, path, res);
                path.pop_back();//删除最后一个字符
            }
        }
    };
    ```

    + C++版本-大佬的间接版本
    ```C++
       class Solution {
        public:
            vector<string> letterCombinations(string digits) {
                if (digits.empty()) return {};
                vector<string> res;
                vector<string> dict{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
                letterCombinationsDFS(digits, dict, 0, "", res);
                return res;
            }
            void letterCombinationsDFS(string& digits, vector<string>& dict, int level, string out, vector<string>& res) {
                if (level == digits.size()) {res.push_back(out); return;}
                string str = dict[digits[level] - '0'];
                for (int i = 0; i < str.size(); ++i) {
                    letterCombinationsDFS(digits, dict, level + 1, out + str[i], res);
                }
            }
        }; 
    ```

- 方法二：迭代法 有点广度遍历树的感觉
    + 这道题也可以用迭代 Iterative 来解，在遍历 digits 中所有的数字时，先建立一个临时的字符串数组t，然后跟上面解法的操作一样，通过数字到 dict 中取出字符串 str，然后遍历取出字符串中的所有字符，再遍历当前结果 res 中的每一个字符串，将字符加到后面，并加入到临时字符串数组t中。取出的字符串 str 遍历完成后，将临时字符串数组赋值给结果 res，具体实现参见代码如下：
    + C++版本
    ```C++
    class Solution {
    public:
        vector<string> letterCombinations(string digits) {
            if(digits.empty())
                return {};
            vector<string> dict{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
            vector<string> res{""};//一定要初始化 不然输出是空
            for(int i = 0; i < digits.size(); ++i){
                vector<string> t;
                string str = dict[digits[i] - '0'];
                for(int j = 0; j < str.size(); ++j){
                    for(string s : res)
                        t.push_back(s + str[j]);
                }
                res = t;
            }
            return res;
        }
    };
    ```