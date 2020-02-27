- [51. N皇后](https://leetcode-cn.com/problems/n-queens/)
- [参考博客](https://github.com/grandyang/leetcode/issues/51)
- 解法一：回溯 + 剪枝
    + 经典的N皇后问题，基本所有的算法书中都会包含的问题。可能有些人对国际象棋不太熟悉，大家都知道中国象棋中最叼的是车，横竖都能走，但是在国际象棋中还有更叼的，就是皇后，不但能横竖走，还能走两个斜线，有如 bug 一般的存在。所以经典的八皇后问题就应运而生了，在一个 8x8 大小的棋盘上如果才能放8个皇后，使得两两之间不能相遇，所谓一山不能容二虎，而这里有八个母老虎，互相都不能相遇。对于这类问题，没有太简便的方法，只能使用穷举法，就是尝试所有的组合，每放置一个新的皇后的时候，必须要保证跟之前的所有皇后不能冲突，若发生了冲突，说明当前位置不能放，要重新找地方，这个逻辑非常适合用递归来做。我们先建立一个长度为 nxn 的全是点的数组 queens，然后从第0行开始调用递归。在递归函数中，我们首先判断当前行数是否已经为n，是的话，说明所有的皇后都已经成功放置好了，所以我们只要将 queens 数组加入结果 res 中即可。否则的话，我们遍历该行的所有列的位置，行跟列的位置都确定后，我们要验证当前位置是否会产生冲突，那么就需要使用一个子函数来判断了，首先验证该列是否有冲突，就遍历之前的所有行，若某一行相同列也有皇后，则冲突返回false；再验证两个对角线是否冲突，就是一些坐标转换，主要不要写错了，若都没有冲突，则说明该位置可以放皇后，放了新皇后之后，再对下一行调用递归即可，注意递归结束之后要返回状态，参见代码如下：
    + 总共八个方向 上下左右 左上右上 左下右下 但是我们是按照从下到上的方式和从左到右的方向遍历的，所以就只需要考察 左上 上 右上三个方向是否满足条件 也即不满足条件的返回 - 剪枝
    ```
    class Solution {
    public:
        vector<vector<string>> solveNQueens(int n) {
            vector<vector<string>> res;
            vector<string> queen(n, string(n, '.'));
            cursion(0, queen, res);
            return res;
        }
        void cursion(int curRow, vector<string>& queen, vector<vector<string>>& res){
            int n = queen.size();
            if(curRow == n){
                res.push_back(queen);
                return ;
            }
            for(int i = 0; i < n; ++i){
                if(isValid(queen, curRow, i)){//满足条件则继续进行递归遍历求解
                    queen[curRow][i] = 'Q';
                    cursion(curRow + 1, queen, res);
                    queen[curRow][i] = '.';//这一步至关重要 递归返回时  始放之前的情况
                }
            }
        }
        bool isValid(vector<string>& queen, int row, int col){//剪枝的条件
            for(int i = 0; i < row; ++i){//遍历垂直上方是否满足条件
                if(queen[i][col] == 'Q')
                    return false;
            }
            for(int i = row - 1, j = col - 1; i >= 0 && j >=0; --i, --j){//遍历左上角是否满足条件
                if(queen[i][j] == 'Q')
                    return false;
            }
            for(int i = row - 1, j = col + 1; i >= 0 && j < queen.size(); --i, ++j){//遍历右上方是否满足条件
                if(queen[i][j] == 'Q')
                    return false;
            }
            return true;
        }
    };
    ```