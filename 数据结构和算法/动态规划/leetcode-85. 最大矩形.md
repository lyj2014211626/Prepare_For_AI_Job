- [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4322667.html)
- [官方解法](https://leetcode-cn.com/problems/maximal-rectangle/solution/zui-da-ju-xing-by-leetcode/)
- 解法一：递增序列栈
    + 此题是之前那道的 [Largest Rectangle in Histogram](https://www.cnblogs.com/grandyang/p/4322653.html) 的扩展，这道题的二维矩阵每一层向上都可以看做一个直方图，输入矩阵有多少行，就可以形成多少个直方图，对每个直方图都调用 Largest Rectangle in Histogram 中的方法，就可以得到最大的矩形面积。那么这道题唯一要做的就是将每一层都当作直方图的底层，并向上构造整个直方图，由于题目限定了输入矩阵的字符只有 '0' 和 '1' 两种，所以处理起来也相对简单。方法是，对于每一个点，如果是 ‘0’，则赋0，如果是 ‘1’，就赋之前的 height 值加上1。具体参见代码如下：
    ```C++
    class Solution {
    public:
        int maximalRectangle(vector<vector<char>>& matrix) {
            int res = 0;
            if(matrix.empty() || matrix[0].empty())
                return 0;
            vector<int> heights(matrix[0].size(), 0);
            for(int i = 0; i < matrix.size(); ++i)
            {
                for(int j = 0; j < matrix[i].size(); ++j)
                {
                    heights[j] = matrix[i][j] == '0' ? 0 :(heights[j] + 1);
                }
                res = max(res, cal_area(heights));
            }
            return res;
        }
        int cal_area(vector<int>& heights)
        {
            heights.push_back(0);
            stack<int> st;
            int res = 0;
            for(int i = 0; i < heights.size(); ++i)
            {
                if(st.empty() || heights[st.top()] < heights[i])
                    st.push(i);
                else
                {
                    int cur = st.top();
                    st.pop();
                    res = max(res, heights[cur] * (st.empty() ? i : i - st.top() - 1));
                    --i;
                }
            }
            return res;
        }
    };
    ```
    + 我们也可以在一个函数内完成，这样代码看起来更加简洁一些，**注意这里的 height 初始化的大小为 n+1**，为什么要多一个呢？这是因为我们只有在当前位置小于等于前一个位置的高度的时候，才会去计算矩形的面积，假如最后一个位置的高度是最高的，那么我们就没法去计算并更新结果 res 了，所以要在最后再加一个高度0，这样就一定可以计算前面的矩形面积了，这跟上面解法子函数中给 height 末尾加一个0是一样的效果，参见代码如下：
    ```C++
    class Solution {
    public:
        int maximalRectangle(vector<vector<char>>& matrix) {
            if (matrix.empty() || matrix[0].empty()) return 0;
            int res = 0, m = matrix.size(), n = matrix[0].size();
            vector<int> height(n + 1); //这里是 n + 1 ->trick
            for (int i = 0; i < m; ++i) {
                stack<int> s;
                for (int j = 0; j < n + 1; ++j) {
                    if (j < n) {
                        height[j] = matrix[i][j] == '1' ? height[j] + 1 : 0;
                    }
                    while (!s.empty() && height[s.top()] >= height[j]) {
                        int cur = s.top(); s.pop();
                        res = max(res, height[cur] * (s.empty() ? j : (j - s.top() - 1)));
                    }
                    s.push(j);
                }
            }
            return res;
        }
    };
    ```