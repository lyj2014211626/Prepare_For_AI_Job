- [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4344107.html)
- [官方解法](https://leetcode-cn.com/problems/edit-distance/solution/zi-di-xiang-shang-he-zi-ding-xiang-xia-by-powcai-3/)
- 解法一：自顶向下的递归解法
    + 这道题让求从一个字符串转变到另一个字符串需要的变换步骤，共有三种变换方式，插入一个字符，删除一个字符，和替换一个字符。题目乍眼一看并不难，但是实际上却暗藏玄机，对于两个字符串的比较，一般都会考虑一下用 HashMap 统计字符出现的频率，但是在这道题却不可以这么做，因为字符串的顺序很重要。还有一种比较常见的错误，就是想当然的认为对于长度不同的两个字符串，长度的差值都是要用插入操作，然后再对应每位字符，不同的地方用修改操作，但是其实这样可能会多用操作，因为删除操作有时同时可以达到修改的效果。比如题目中的例子1，当把 horse 变为 rorse 之后，之后只要删除第二个r，跟最后一个e，就可以变为 ros。实际上只要三步就完成了，因为删除了某个字母后，原来左右不相连的字母现在就连一起了，有可能刚好组成了需要的字符串。所以在比较的时候，要尝试三种操作，因为谁也不知道当前的操作会对后面产生什么样的影响。对于当前比较的两个字符 word1[i] 和 word2[j]，若二者相同，一切好说，直接跳到下一个位置。若不相同，有三种处理方法，首先是直接插入一个 word2[j]，那么 word2[j] 位置的字符就跳过了，接着比较 word1[i] 和 word2[j+1] 即可。第二个种方法是删除，即将 word1[i] 字符直接删掉，接着比较 word1[i+1] 和 word2[j] 即可。第三种则是将 word1[i] 修改为 word2[j]，接着比较 word1[i+1] 和 word[j+1] 即可。分析到这里，就可以直接写出递归的代码，但是很可惜会 Time Limited Exceed，所以必须要优化时间复杂度，需要去掉大量的重复计算，这里使用记忆数组 memo 来保存计算过的状态，从而可以通过 OJ，注意这里的 insertCnt，deleteCnt，replaceCnt 仅仅是表示当前对应的位置分别采用了插入，删除，和替换操作，整体返回的最小距离，后面位置的还是会调用递归返回最小的，参见代码如下：
    ```C++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            int m = word1.size();
            int n = word2.size();
            vector<vector<int>>dp(m, vector<int>(n, 0));//注意大小
            return cursion(word1, 0, word2, 0, dp); 
        }
        int cursion(string& word1, int i, string& word2, int j, vector<vector<int>>& dp)
        {
            if(i == word1.size())
                return word2.size() - j;
            if(j == word2.size())
                return word1.size() - i;
            if(dp[i][j] > 0)
                return dp[i][j];
            int res = 0;
            if(word1[i] == word2[j])
                return cursion(word1, i + 1, word2, j + 1, dp);
            else
            {
                int insertNum = cursion(word1, i, word2, j + 1, dp);
                int deleteNum = cursion(word1, i + 1, word2, j, dp);
                int changeNum = cursion(word1, i + 1, word2, j + 1, dp);
                res = min(min(insertNum, deleteNum), changeNum) + 1;
            }
            dp[i][j] = res;
            return res;
        }
    };
    ```

- 解法二：自底向上的动态规划
    + **根据以往的经验，对于字符串相关的题目且求极值的问题，十有八九都是用动态规划 Dynamic Programming 来解，**这道题也不例外。其实解法一的递归加记忆数组的方法也可以看作是 DP 的递归写法。这里需要维护一个二维的数组 dp，其大小为 mxn，m和n分别为 word1 和 word2 的长度。dp[i][j] 表示从 word1 的前i个字符转换到 word2 的前j个字符所需要的步骤。先给这个二维数组 dp 的第一行第一列赋值，这个很简单，因为第一行和第一列对应的总有一个字符串是空串，于是转换步骤完全是另一个字符串的长度。跟以往的 DP 题目类似，难点还是在于找出状态转移方程，可以举个例子来看，比如 word1 是 "bbc"，word2 是 "abcd"，可以得到 dp 数组如下：
    ```
          Ø a b c d
        Ø 0 1 2 3 4
        b 1 1 1 2 3
        b 2 2 1 2 3
        c 3 3 2 1 2
    ```
    + 通过观察可以发现，当 word1[i] == word2[j] 时，dp[i][j] = dp[i - 1][j - 1]，其他情况时，dp[i][j] 是其左，左上，上的三个值中的最小值加1，其实这里的左，上，和左上，分别对应的增加，删除，修改操作，具体可以参见解法一种的讲解部分，那么可以得到状态转移方程为：
    ```
    dp[i][j] =      /    dp[i - 1][j - 1]                                      if word1[i - 1] == word2[j - 1]

                    \    min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1])) + 1            else
    ```

    ```C++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            int m = word1.size();
            int n = word2.size();
            vector<vector<int>>dp(m + 1, vector<int>(n + 1, 0));
            for(int i = 0; i <=m; ++i)  //初始化
                dp[i][0] = i;
            for(int i = 0; i <= n; ++i) //初始化
                dp[0][i] = i;
            for(int i = 1; i <= m; ++i) //注意下标的范围
            {
               for(int j = 1; j <= n; ++j)
               {
                   if(word1[i - 1] == word2[j - 1])
                       dp[i][j] = dp[i - 1][j - 1];
                   else
                       dp[i][j] = min(min(dp[i - 1][j - 1], dp[i - 1][j]), dp[i][j - 1]) + 1;
               }
            }
            return dp[m][n];
        }
    };
    ```

- 用一维动态数组：[参考博客](https://www.dreamxu.com/books/dsa/dp/edit-distance.html)
    + 还是以 a = "fxy", b = "fab" 为例，例如计算 d[1][3], 也就是下图中的绿色方块， 我们需要知道的值只需 3 个，下图中蓝色方块的值
    + ![Alt text](https://www.dreamxu.com/books/dsa/dp/images/2014-11-05_134029.svg)
    + 进一步分析，我们知道，当计算 d[1] 这行的时候，我们只需知道 d[0] 这行的值， 同理我们计算当前行的时候只需知道上一行就可以了。再进一步分析，其实我们只需要一行就可以了，每次计算的时候我们需要的 3 个值，**其中上边和左边的值我们可以直接得到，坐上角的值需要临时变量**（如下代码使用 old）来记录
    + [这里给出最长公共子序列的空间优化](https://blog.csdn.net/qq_37341466/article/details/83036039)
    ```C++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            int m = word1.size();
            int n = word2.size();
            int old;
            int tmp;
            vector<int> dp(n + 1, 0);
            for(int i = 0; i <= n; ++i)
                dp[i] = i;
            for(int i = 1; i <= m; ++i)
            {
                old = i - 1;    //这里相当于记录上一层数组的第一个数值
                dp[0] = i;     //这里的dp[0]=i值是第i行所决定的 和列的值无关
                for(int j = 1; j <= n; ++j)
                {
                    tmp = dp[j];    //保存上一层的中间结果 即dp[i-1][j-1]
                    if(word1[i - 1] == word2[j - 1])
                        dp[j] = old;
                    else
                        dp[j] = min(min(dp[j], dp[j - 1]), old) + 1;
                    
                    old = tmp;
                }
               
            }
            return dp[n];
        }
    };
    ```