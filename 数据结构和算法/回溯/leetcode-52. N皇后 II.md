- [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)
- [参考博客](https://github.com/grandyang/leetcode/issues/52)
- 解法：回溯+剪枝
    + 这道题是之前那道 N-Queens 的延伸，说是延伸其实我觉得两者顺序应该颠倒一样，上一道题比这道题还要稍稍复杂一些，两者本质上没有啥区别，都是要用回溯法 Backtracking 来解，如果理解了之前那道题的思路，此题只要做很小的改动即可，不再需要求出具体的皇后的摆法，只需要每次生成一种解法时，计数器加一即可，代码如下：
    ```
    class Solution {
    public:
        int totalNQueens(int n) {
            int res = 0;
            vector<string> queen(n, string(n, '.'));
            cursion(0, queen, res);
            return res;
        }
        void cursion(int curRow, vector<string>& queen, int &res){
            int n = queen.size();
            if(curRow == n){
                ++res;//这里改动了一下
                return ;
            }
            for(int i = 0; i < n; ++i){
                if(isVald(queen, curRow, i)){
                    queen[curRow][i] = 'Q';
                    cursion(curRow + 1, queen, res);
                    queen[curRow][i] = '.';
                }
            }
        }
        bool isVald(vector<string>& queen, int row, int col){
            for(int i = 0; i < row; ++i){
                if(queen[i][col] == 'Q')
                    return false;
            }
            for(int i = row -1, j = col - 1; i >=0 && j >=0; --i, --j){
                if(queen[i][j] == 'Q')
                    return false;
            }
            for(int i = row - 1, j = col + 1; i >= 0 && j < queen.size(); --i, ++j){
                if(queen[i][j] == 'Q')
                    return false;
            }
            return true;
        }
    };
    ```