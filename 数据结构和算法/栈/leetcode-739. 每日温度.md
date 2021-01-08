- [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
- [参考解法](https://www.cnblogs.com/grandyang/p/8097513.html)
- [官方解法](https://leetcode-cn.com/problems/daily-temperatures/solution/mei-ri-wen-du-by-leetcode-solution/)
- 解法一：单调栈
    + 这道题是字节的笔试题。
    + [相似的题目](https://blog.csdn.net/bxw1992/article/details/77856304)
    + 这道题给了我们一个数组，让我们找下一个比当前数字大的数字的距离，我们研究一下题目中给的例子，发现数组是无序的，所以没法用二分法快速定位下一个大的数字，那么最先考虑的方法就是暴力搜索了，写起来没有什么难度，但是OJ并不答应。实际上这道题应该使用递减栈Descending Stack来做，栈里只有递减元素，思路是这样的，我们遍历数组，如果栈不空，且当前数字大于栈顶元素，那么如果直接入栈的话就不是递减栈了，所以我们取出栈顶元素，那么由于当前数字大于栈顶元素的数字，而且一定是第一个大于栈顶元素的数，那么我们直接求出下标差就是二者的距离了，然后继续看新的栈顶元素，直到当前数字小于等于栈顶元素停止，然后将数字入栈，这样就可以一直保持递减栈，且每个数字和第一个大于它的数的距离也可以算出来了，参见代码如下：
    ```C++
    class Solution {
    public:
        vector<int> dailyTemperatures(vector<int>& T) {
            vector<int> res(T.size(), 0);
            stack<int> s;
            s.push(0);
            for(int i = 1; i < T.size(); ++i)
            {
                while(!s.empty() && T[i] > T[s.top()])
                {
                    res[s.top()] = i - s.top();
                    s.pop();
                }
                s.push(i);
            }
            return res;
        }
    };
    ```