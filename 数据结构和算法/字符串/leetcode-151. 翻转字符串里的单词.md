- [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4606676.html)
- [官方解法](https://leetcode-cn.com/problems/reverse-words-in-a-string/solution/fan-zhuan-zi-fu-chuan-li-de-dan-ci-by-leetcode-sol/)
- 解法一：用字符流
    ```C++
    class Solution {
    public:
        string reverseWords(string s) {
            istringstream sin(s);
            string res = "";
            string t;
            while(sin>>t)
                res = t + " " + res;
            // if(!res.empty())
            //     res.pop_back();
            if(!res.empty() && res[res.size() - 1] == ' ')
                res.erase(res.end() - 1, res.end());
            return res;
        }
    };
    ```
- 解法二
    + 先对整个字符串反转，然后对每个单词反转。
    ```C++
    class Solution {
    public:
        string reverseWords(string s) {
            int storeIndex = 0, n = s.size();
            reverse(s.begin(), s.end());
            for (int i = 0; i < n; ++i) {
                if (s[i] != ' ') {
                    if (storeIndex != 0) s[storeIndex++] = ' ';
                    int j = i;
                    while (j < n && s[j] != ' ') s[storeIndex++] = s[j++];
                    reverse(s.begin() + storeIndex - (j - i), s.begin() + storeIndex);
                    i = j;
                }
            }
            s.resize(storeIndex);
            return s;
        }
    }; 
    ```