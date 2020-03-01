- [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)
- [参考博客](https://github.com/grandyang/leetcode/issues/39)
- [官方解答](https://leetcode-cn.com/problems/combination-sum/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-2/)
- 解法一：回溯 + 剪枝
    + ![Alt Text](https://pic.leetcode-cn.com/ade93b4f0678b2b1385ad1362ff426ce0a5a800a5b0ae07dfb65f58677374559-39-3.png)
    + 像这种结果要求返回所有符合要求解的题十有八九都是要利用到递归，而且解题的思路都大同小异，相类似的题目有 Path Sum II，Subsets II，Permutations，Permutations II，Combinations 等等，如果仔细研究这些题目发现都是一个套路，都是需要另写一个递归函数，这里我们新加入三个变量，start 记录当前的递归到的下标，out 为一个解，res 保存所有已经得到的解，每次调用新的递归函数时，此时的 target 要减去当前数组的的数，具体看代码如下
    + **去重复**:在搜索的时候，需要设置搜索起点的下标 begin ，由于一个数可以使用多次，**下一层的结点从这个搜索起点开始搜索**；在搜索起点 begin 之前的数因为以前的分支搜索过了，所以一定会产生重复

    + C++ 实现
    ```
    class Solution {
    public:
        vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
            vector<int> t;
            vector<vector<int>> res;
            cursion(candidates, target, t, 0, res);
            return res;
        }
        void cursion(vector<int>& candidate, int target, vector<int>& t, int begin,vector<vector<int>>& res)
        {
            if(target < 0)//不符合条件直接返回 这里不用判断当前节点是否越界
                return;
            if(target == 0)
            {
                res.push_back(t);
                return;
            }
            for(int i = begin; i < candidate.size(); ++i)
            {
                t.push_back(candidate[i]);
                cursion(candidate, target - candidate[i], t, i, res);//注意这里的变量 i 相当于记录递归到当前数字的下标 为了防出现重复的组合情况 这里也相当于是剪枝啦
                t.pop_back();
            }
        }
    };
    ```