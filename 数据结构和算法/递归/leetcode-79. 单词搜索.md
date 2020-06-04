- [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)
- [参考博客](https://github.com/grandyang/leetcode/issues/79)
- [官方解答](https://leetcode-cn.com/problems/word-search/solution/zai-er-wei-ping-mian-shang-shi-yong-hui-su-fa-pyth/)
- 解法一：回溯
    + 这道题是典型的深度优先遍历 DFS 的应用，原二维数组就像是一个迷宫，可以上下左右四个方向行走，我们以二维数组中每一个数都作为起点和给定字符串做匹配，我们还需要一个和原数组等大小的 visited 数组，是 bool 型的，用来记录当前位置是否已经被访问过，因为题目要求一个 cell 只能被访问一次。如果二维数组 board 的当前字符和目标字符串 word 对应的字符相等，则对其上下左右四个邻字符分别调用 DFS 的递归函数，只要有一个返回 true，那么就表示可以找到对应的字符串，否则就不能找到，具体看代码实现如下：
    + 这里就相等于对二维数据中的每个元素进行dfs深度遍历即可，然后回溯修改变量。
    ```C++
    class Solution {
    public:
        bool exist(vector<vector<char>>& board, string word) {
            if(board.empty() || board[0].empty())
                return false;
            int m = board.size();
            int n = board[0].size();
            vector<vector<bool>> visited(m, vector<bool>(n));
            for(int i = 0; i < m; ++i)
            {
                for(int j = 0; j < n; ++j)
                {
                    if(search(board, word, 0, i, j, visited))//数据中每一个元素作为dfs的根节点
                        return true;
                }
            }
            return false;
        }
        bool search(vector<vector<char>>& board, string& word, int idx, int i, int j, vector<vector<bool>>& visited)
        {
            if(idx == word.size())
                return true;
            int m = board.size();
            int n = board[0].size();
            if(i < 0 || i >= m || j < 0 || j >= n || visited[i][j] || word[idx] != board[i][j])
                return false;
            visited[i][j] = true;//修改变量
            bool res =  search(board, word, idx + 1, i - 1, j, visited) ||
                        search(board, word, idx + 1, i, j - 1, visited) ||
                        search(board, word, idx + 1, i + 1, j, visited) ||
                        search(board, word, idx + 1, i, j + 1, visited);
            visited[i][j] = false;//修改变量 回溯
            return res;
        }
    };
    ```

- 我们还可以不用 visited 数组，直接对 board 数组进行修改，将其遍历过的位置改为井号，记得递归调用完后需要恢复之前的状态，参见代码如下：
```C++
    class Solution {
    public:
        bool exist(vector<vector<char>>& board, string word) {
            if (board.empty() || board[0].empty()) return false;
            int m = board.size(), n = board[0].size();
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (search(board, word, 0, i, j)) return true;
                }
            }
            return false;
        }
        bool search(vector<vector<char>>& board, string word, int idx, int i, int j) {
            if (idx == word.size()) return true;
            int m = board.size(), n = board[0].size();
            if (i < 0 || j < 0 || i >= m || j >= n || board[i][j] != word[idx]) return false;    
            char c = board[i][j];
            board[i][j] = '#';
            bool res = search(board, word, idx + 1, i - 1, j) 
                     || search(board, word, idx + 1, i + 1, j)
                     || search(board, word, idx + 1, i, j - 1)
                     || search(board, word, idx + 1, i, j + 1);
            board[i][j] = c;
            return res;
        }
    };
```