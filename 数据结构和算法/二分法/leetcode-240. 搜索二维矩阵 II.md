- [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4669134.html)
- [官方解法](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/solution/sou-suo-er-wei-ju-zhen-ii-by-leetcode-2/)
- 解法一：二分法
    + 如果我们观察题目中给的那个例子，我们可以发现有两个位置的数字很有特点，左下角和右上角的数。左下角的18，往上所有的数变小，往右所有数增加，那么我们就可以和目标数相比较，如果目标数大，就往右搜，如果目标数小，就往上搜。这样就可以判断目标数是否存在。当然我们也可以把起始数放在右上角，往左和下搜，停止条件设置正确就行。代码如下：
    ```C++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            if(matrix.empty() || matrix[0].empty())
                return false;
            int m = matrix.size();
            int n = matrix[0].size();
            if(target < matrix[0][0] || target > matrix[m - 1][n - 1])
                return false;
            int x = m - 1;  //选择左下角数进行比较
            int y = 0;
            while(true)
            {
                if(target > matrix[x][y])
                    ++y;
                else if(target < matrix[x][y])
                    --x;
                else 
                    return true;
                if(x < 0 || y >= n)
                    break;
            }
            return false;
        }
    };
    ```

    + 该代码是从矩阵的右上角进行比较
    ```C++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            if(matrix.empty() || matrix[0].empty())
                return false;
            int m = matrix.size();
            int n = matrix[0].size();
            if(target < matrix[0][0] || target > matrix[m - 1][n - 1])
                return false;
            int x = 0;  //右上角进行比较
            int y = n - 1;
            while(true)
            {
                if(target > matrix[x][y])
                    ++x;
                else if(target < matrix[x][y])
                    --y;
                else 
                    return true;
                if(x >= m || y < 0)
                    break;
            }
            return false;
        }
    };
    ```