- [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
- [参考博客](https://github.com/grandyang/leetcode/issues/5)
- [官方解答](https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode/)
- 解法一：动态规划
    + 此题还可以用动态规划 Dynamic Programming 来解，根 Palindrome Partitioning II 的解法很类似，我们维护一个二维数组 dp，其中 dp[i][j] 表示字符串区间 [i, j] 是否为回文串，当 i = j 时，只有一个字符，肯定是回文串，如果 i = j + 1，说明是相邻字符，此时需要判断 s[i] 是否等于 s[j]，如果i和j不相邻，即 i - j >= 2 时，除了判断 s[i] 和 s[j] 相等之外，dp[i + 1][j - 1] 若为真，就是回文串，通过以上分析，可以写出递推式如下：

dp[i, j] = 1                                               if i == j （case 1）

           = s[i] == s[j]                                if j = i + 1（case 2）

           = s[i] == s[j] && dp[i + 1][j - 1]    if j > i + 1        （case 3）

这里有个有趣的现象就是如果我把下面的代码中的二维数组由 int 改为 vector<vector> 后，就会超时，这说明 int 型的二维数组访问执行速度完爆 std 的 vector 啊，所以以后尽可能的还是用最原始的数据类型吧。
```
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.empty()) return "";//空串判断
        int n = s.size();
        int left = 0;//字符串的左边界
        int len = 1;//长度
        //vector<vector<int>> dp(n, vector<int>(n, 0));
        int dp[n][n] = {0};
        for(int j = 0; j < n; ++j)
        {
            dp[j][j] = 1;
            for(int i = 0; i < j ; ++i)
            {
                dp[i][j] = (s[i] == s[j]) && (j - i < 2 || dp[i + 1][j - 1]);
                if(dp[i][j] == 1 && j - i + 1 > len)
                {
                    len = j - i + 1;
                    left = i;
                }
            }
        }
        return s.substr(left, len);
    }
}; 
```

- 解法二：中心扩散法
    + 这道题让我们求最长回文子串，首先说下什么是回文串，就是正读反读都一样的字符串，比如 "bob", "level", "noon" 等等。那么最长回文子串就是在一个字符串中的那个最长的回文子串。LeetCode 中关于回文串的题共有五道，除了这道，其他的四道为 Palindrome Number，Validate Palindrome，Palindrome Partitioning，Palindrome Partitioning II，我们知道传统的验证回文串的方法就是两个两个的对称验证是否相等，那么对于找回文字串的问题，就要以每一个字符为中心，像两边扩散来寻找回文串，这个算法的时间复杂度是 O(n*n)，可以通过 OJ，就是要注意奇偶情况，由于回文串的长度可奇可偶，比如 "bob" 是奇数形式的回文，"noon" 就是偶数形式的回文，两种形式的回文都要搜索，对于奇数形式的，我们就从遍历到的位置为中心，向两边进行扩散，对于偶数情况，我们就把当前位置和下一个位置当作偶数行回文的最中间两个字符，然后向两边进行搜索，参见代码如下：
    + C++版本
    ```
    class Solution {
    public:
        string longestPalindrome(string s) {
            if (s.empty()) return "";
            int start = 0;
            int max_len = 0;
            for(int i = 0; i < s.size(); ++i)
            {
                cursion(s, i, i, start, max_len);//遍历奇数回文串的情况
                cursion(s, i, i + 1, start, max_len);//遍历偶数回文串的情况
            }
            return s.substr(start, max_len);
        }
        void cursion(string s, int left, int right, int& start, int& max_len)
        {
            while(left >= 0 && right < s.size() && s[left] == s[right])
            {
                --left;
                ++right;
            }
            if(right - left - 1 > max_len)
            {
                start = left + 1;
                max_len = right - left - 1;
            }
        }
    };
    ```

    + 简化
    ```
    class Solution {
    public:
        string longestPalindrome(string s) {
            if (s.size() < 2) return s;
            int n = s.size(), maxLen = 0, start = 0;
            for (int i = 0; i < n;) {
                if (n - i <= maxLen / 2) break;
                int left = i, right = i;
                while (right < n - 1 && s[right + 1] == s[right]) ++right;
                i = right + 1;
                while (right < n - 1 && left > 0 && s[right + 1] == s[left - 1]) {
                    ++right; --left;
                }
                if (maxLen < right - left + 1) {
                    maxLen = right - left + 1;
                    start = left;
                }
            }
            return s.substr(start, maxLen);
        }
    };
    ```
- 解法三 ：马拉车算法 Manacher's Algorithm
    + 最后要来的就是大名鼎鼎的马拉车算法 Manacher's Algorithm，这个算法的神奇之处在于将时间复杂度提升到了 O(n) 这种逆天的地步，而算法本身也设计的很巧妙，很值得我们掌握，参见我另一篇专门介绍马拉车算法的博客 [Manacher's Algorithm 马拉车算法](https://www.cnblogs.com/grandyang/p/4475985.html)，代码实现如下:
    + C++版本
    ```
    class Solution {
    public:
        string longestPalindrome(string s) {
            string t ="$#";
            for (int i = 0; i < s.size(); ++i) {
                t += s[i];
                t += '#';
            }
            int p[t.size()] = {0}, id = 0, mx = 0, resId = 0, resMx = 0;
            for (int i = 1; i < t.size(); ++i) {
                p[i] = mx > i ? min(p[2 * id - i], mx - i) : 1;
                while (t[i + p[i]] == t[i - p[i]]) ++p[i];
                if (mx < i + p[i]) {
                    mx = i + p[i];
                    id = i;
                }
                if (resMx < p[i]) {
                    resMx = p[i];
                    resId = i;
                }
            }
            return s.substr((resId - resMx) / 2, resMx - 1);
        }
    };
    ```