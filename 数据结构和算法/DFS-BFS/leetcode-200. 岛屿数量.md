- [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
- [参考写法](https://www.cnblogs.com/grandyang/p/4402656.html)
- [官方写法](https://leetcode-cn.com/problems/number-of-islands/solution/dao-yu-shu-liang-by-leetcode/)
- 解法一：DFS
    + 这道求岛屿数量的题的本质是求矩阵中连续区域的个数，很容易想到需要用深度优先搜索 DFS 来解，我们需要建立一个 visited 数组用来记录某个位置是否被访问过，对于一个为 ‘1’ 且未被访问过的位置，递归进入其上下左右位置上为 ‘1’ 的数，将其 visited 对应值赋为 true，继续进入其所有相连的邻位置，这样可以将这个连通区域所有的数找出来，并将其对应的 visited 中的值赋 true，找完相邻区域后，将结果 res 自增1，然后再继续找下一个为 ‘1’ 且未被访问过的位置，以此类推直至遍历完整个原数组即可得到最终结果，代码如下：
    ```C++
    class Solution {
    public:
        int numIslands(vector<vector<char>>& grid) {
            if(grid.empty() || grid[0].empty())
                return 0;
            int m = grid.size();
            int n = grid[0].size();
            vector<vector<bool>> visit(m, vector<bool>(n, 0));
            int res = 0;
            for(int i = 0; i < m; ++i)
            {
                for(int j = 0; j < n; ++j)
                {
                    if(grid[i][j] == '0' || visit[i][j])
                        continue;
                    cursion(i ,j, grid, visit);
                    ++res;
                }
            }
            return res;
        }
        void cursion(int i, int j, vector<vector<char>>& grid, vector<vector<bool>>& visit)
        {
            int m = grid.size();
            int n = grid[0].size();
            if(i < 0 || i >= m || j < 0 || j >= n || visit[i][j] || grid[i][j] == '0')
                return;
            visit[i][j] = true;
            cursion(i - 1, j, grid, visit);
            cursion(i + 1, j, grid, visit);
            cursion(i, j - 1, grid, visit);
            cursion(i, j + 1, grid, visit);
        }
    };
    ```

- 解法二：广度遍历
    + 当然，这种类似迷宫遍历的题目 DFS 和 BFS 两对好基友肯定是形影不离的，那么 BFS 搞起。其实也很简单，就是在遍历到 ‘1’ 的时候，且该位置没有被访问过，那么就调用一个 BFS 即可，借助队列 queue 来实现，现将当前位置加入队列，然后进行 while 循环，将队首元素提取出来，并遍历其周围四个位置，若没有越界的话，就将 visited 中该邻居位置标记为 true，并将其加入队列中等待下次遍历即可，参见代码如下：
    
    ```C++
    class Solution {
    public:
        int numIslands(vector<vector<char>>& grid) {
            if(grid.empty() || grid[0].empty())
                return 0;
            int m = grid.size();
            int n = grid[0].size();
            vector<vector<bool>> visit(m, vector<bool>(n, 0));
            int res = 0;
            vector<int> dirx{-1, 1, 0, 0};
            vector<int> diry{0, 0, -1, 1};
            for(int i = 0; i < m; ++i)
            {
                for(int j = 0; j < n; ++j)
                {
                    if(grid[i][j] == '0' || visit[i][j])
                        continue;
                    ++res;
                    queue<int> qu {{i * n + j}};
                    while(!qu.empty())
                    {
                        int t = qu.front();
                        qu.pop();
                        for(int k = 0; k < 4; ++k)
                        {
                            int x = t / n + dirx[k];
                            int y = t % n + diry[k];
                            if(x < 0 || x >= m || y < 0 || y >= n || visit[x][y] || grid[x][y] == '0')
                                continue;
                            visit[x][y] = true;
                            qu.push(x * n + y);
                        }
                    }
                }
            }
            return res;
        }
    };
    ```