- [**74. 搜索二维矩阵**](https://leetcode-cn.com/problems/search-a-2d-matrix/)
- [参考答案](https://github.com/grandyang/leetcode/issues/74)
- 解法一
    + 进行**二次二分查找**。第一次是在第一列进行二分查找，找到该数所在的列，然后再在对应的行数据上再进行一次二分查找。
    ```
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            if(matrix.empty() || matrix[0].empty()) //边界条件的考虑
                return false;
            int right = matrix.size();
            int left = 0;
            while(left < right){
                int mid = left + ((right - left) >> 1);// 注意括号
                if(matrix[mid][0] == target){
                    return true;
                }
                else if(matrix[mid][0] < target)
                    left = mid + 1;
                else
                    right = mid;
            }
            int find_row = right > 0 ? (right - 1) : right;//这里注意 查找有可能越界
            left = 0;
            right = matrix[find_row].size();
            while(left < right){
                int mid = left + ((right - left) >> 1);
                if(matrix[find_row][mid] == target)
                    return true;
                if(matrix[find_row][mid] < target)
                    left = mid + 1;
                else
                    right = mid;
            }
            return false;
        }
    };
    ```

- 解法二
    + 然这道题也可以使用**一次二分查找法**，如果我们按**S型遍历该二维数组**，可以得到一个有序的一维数组，只需要用一次二分查找法，而**关键就在于坐标的转换**，如何把二维坐标和一维坐标转换是关键点，把一个长度为n的一维数组转化为 mn 的二维数组 (mn = n)后，那么原一维数组中下标为i的元素将出现在二维数组中的 **[i/n][i%n]**的位置，有了这一点，代码很好写出来了：
    ```
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            if(matrix.empty() || matrix[0].empty())
                return false;
            int m = matrix.size();
            int n = matrix[0].size();
            int left = 0;
            int right = m * n;
            while(left < right){
                int mid = left + ((right - left) >> 1);
                if(matrix[mid / n][mid % n] == target)//这里注意分母都是n, 一个求商，一个取余
                    return true;
                else if(matrix[mid / n][mid % n] < target)
                    left = mid + 1;
                else
                    right = mid;
            }
            return false;
        }
    };
    ```

- 解法三
    + 这道题其实也可以不用二分搜索法，直接使用**双指针**也是可以的，i指向0，j指向列数，这样第一个被验证的数就是二维数组右上角的数字，假如这个数字等于 target，直接返回 true；若大于 target，说明要减小数字，则列数j自减1；若小于 target，说明要增加数字，行数i自增1。若 while 循环退出了还是没找到 target，直接返回 false 即可，参见代码如下：
    ```
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            if(matrix.empty() || matrix[0].empty())
                return false;
            int i = 0;
            int j = matrix[0].size() - 1;
            while(i < matrix.size() && j >=0){
                if(matrix[i][j] == target)
                    return true;
                if(matrix[i][j] < target)
                    ++i;
                else
                    --j;
            }
            return false;
        }
    };
    ```