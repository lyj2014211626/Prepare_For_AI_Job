- [46. 全排列](https://leetcode-cn.com/problems/permutations/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4358848.html)
- [官方解法](https://leetcode-cn.com/problems/permutations/solution/quan-pai-lie-by-leetcode/)
- 解法一：回溯法
    + 经典的回溯问题，深度dfs进行遍历组合树即可。需要用一个visited数组来标记访问的下标，下面是我自己手写的代码。
    ```C++
    class Solution {
    public:
        vector<vector<int>> permute(vector<int>& nums) {
            if(nums.empty())
                return {};
            vector<vector<int>> res;
            vector<int> tmp;
            vector<bool>visited(nums.size(), false);
            cursion(nums, 0, visited, tmp, res);
            return res;
        }
        void cursion(vector<int>& nums, int level, vector<bool>& visited, vector<int>& tmp, vector<vector<int>>& res)
        {
            if(level == nums.size())
            {
                res.push_back(tmp);
                return;
            }
            for(int i = 0; i < nums.size(); ++i)
            {
                if(!visited[i])
                {
                    visited[i] = true;
                    tmp.push_back(nums[i]);
                    cursion(nums, level + 1, visited, tmp, res);
                    tmp.pop_back();
                    visited[i] = false;
                }
            }
        }
    };
    ```

    + 参考博客的思路
    + 这道题是求全排列问题，给的输入数组没有重复项，这跟之前的那道 [Combinations](https://www.cnblogs.com/grandyang/p/4332522.html) 和类似，解法基本相同，但是不同点在于那道不同的数字顺序只算一种，是一道典型的组合题，而此题是求全排列问题，还是**用递归 DFS **来求解。这里需要用到一个 visited 数组来标记某个数字是否访问过**，然后在 DFS 递归函数从的循环应从头开始，而不是从 level 开始，这是和 Combinations 不同的地方，其余思路大体相同。这里再说下 level 吧，其本质是记录当前已经拼出的个数，一旦其达到了 nums 数组的长度，说明此时已经是一个全排列了，因为再加数字的话，就会超出。还有就是，为啥这里的 level 要从0开始遍历，因为这是求全排列，每个位置都可能放任意一个数字，这样会有个问题，数字有可能被重复使用，由于全排列是不能重复使用数字的，所以需要用一个 visited 数组来标记某个数字是否使用过，代码如下：
    ```C++
    class Solution {
    public:
        vector<vector<int>> permute(vector<int>& num) {
            vector<vector<int>> res;
            vector<int> out, visited(num.size(), 0);
            permuteDFS(num, 0, visited, out, res);
            return res;
        }
        void permuteDFS(vector<int>& num, int level, vector<int>& visited, vector<int>& out, vector<vector<int>>& res) {
            if (level == num.size()) {res.push_back(out); return;}
            for (int i = 0; i < num.size(); ++i) {
                if (visited[i] == 1) continue;
                visited[i] = 1;
                out.push_back(num[i]);
                permuteDFS(num, level + 1, visited, out, res);
                out.pop_back();
                visited[i] = 0;
            }
        }
    };
    ```

- 解法二：交换二个数的位置
    + 还有一种递归的写法，更简单一些，这里是每次交换 num 里面的两个数字，经过递归可以生成所有的排列情况。这里你可能注意到，为啥在递归函数中， push_back() 了之后没有返回呢，而解法一或者是 Combinations 的递归解法在更新结果 res 后都 return 了呢？其实如果你仔细看代码的话，此时 start 已经大于等于 num.size() 了，而下面的 for 循环的i是从 start 开始的，根本就不会执行 for 循环里的内容，就相当于 return 了，博主偷懒就没写了。但其实为了避免混淆，最好还是加上，免得和前面的搞混了，代码如下：
    ```C++
    class Solution {
    public:
        vector<vector<int>> permute(vector<int>& num) {
            vector<vector<int>> res;
            permuteDFS(num, 0, res);
            return res;
        }
        void permuteDFS(vector<int>& num, int start, vector<vector<int>>& res) {
            if (start >= num.size()) res.push_back(num);
            for (int i = start; i < num.size(); ++i) {
                swap(num[start], num[i]);
                permuteDFS(num, start + 1, res);
                swap(num[start], num[i]);
            }
        }
    };
    ```