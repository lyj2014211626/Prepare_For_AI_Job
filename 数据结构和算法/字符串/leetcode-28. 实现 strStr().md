- [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)
- [参考博客](https://github.com/grandyang/leetcode/issues/28)
- [官方解答](https://leetcode-cn.com/problems/implement-strstr/solution/shi-xian-strstr-by-leetcode/)
- 解法一：
    + 这道题让我们在一个字符串中找另一个字符串第一次出现的位置，那首先要做一些判断，**如果子字符串为空，则返回0，如果子字符串长度大于母字符串长度，则返回 -1**。然后开始遍历母字符串，这里并不需要遍历整个母字符串，而是遍历到剩下的长度和子字符串相等的位置即可，这样可以提高运算效率。然后对于每一个字符，都遍历一遍子字符串，一个一个字符的对应比较，如果对应位置有不等的，则跳出循环，如果一直都没有跳出循环，则说明子字符串出现了，则返回起始位置即可，代码如下：
    + C++代码， 自己写的，很冗余
    ```
    class Solution {
    public:
        int strStr(string haystack, string needle) {
            int m = haystack.size();
            int n = needle.size();
            int i = 0;
            int j = 0;
            if(m < n)//边界条件
                return -1;
            if(n == 0)
                return 0;
            while(i <= m - n)//剪枝处理，剩下如果小于匹配字符串的长度就跳出
            {
                if(haystack[i] == needle[j])
                {
                    int same_num = 0;
                    int k = 0;
                    for(; k < n && i + k < m; ++k)
                    {
                        if(haystack[i + k] == needle[k])
                        {
                            ++same_num;
                        }
                        else
                        {
                            ++i;
                            break;
                        }
                    }
                    if(same_num == n)
                        return i;
                }
                else
                {
                    ++i;
                }
            }
            return -1;
        }
    };
    ```

    + 官方解法的简洁版
    ```
    class Solution {
    public:
        int strStr(string haystack, string needle) {
            if (needle.empty()) return 0;
            int m = haystack.size(), n = needle.size();
            if (m < n) return -1;
            for (int i = 0; i <= m - n; ++i) {
                int j = 0;
                for (j = 0; j < n; ++j) {
                    if (haystack[i + j] != needle[j]) break;
                }
                if (j == n) return i;
            }
            return -1;
        }
    };
    ```