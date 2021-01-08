- [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4419386.html)
- [官方解法](https://leetcode-cn.com/problems/combination-sum-ii/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-3/)
- 解法一：回溯+剪枝
    + 这道题跟之前那道 Combination Sum 本质没有区别，只需要改动一点点即可，之前那道题给定数组中的数字可以重复使用，而这道题不能重复使用，只需要在之前的基础上修改两个地方即可，首先在递归的 for 循环里加上 if (i > start && num[i] == num[i - 1]) continue; 这样可以防止 res 中出现重复项，然后就在递归调用 helper 里面的参数换成 i+1，这样就不会重复使用数组中的数字了，代码如下：
    ```C++
    class Solution {
    public:
        void help(vector<int>& candidates, int target, int begin, vector<int>&t, vector<vector<int>>& res)
        {
            if(target < 0)
                return;
            if(target == 0)
            {
                res.push_back(t);
                return;
            }
            for(int i = begin; i < candidates.size(); ++i)
            {
                 if(i > begin && candidates[i] == candidates[i - 1])//这个条件很重要 一个也不能少
                    continue;
                 t.push_back(candidates[i]);
                 help(candidates, target - candidates[i], i + 1, t, res);
                 t.pop_back();
                 
            }
        }
        vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
            vector<vector<int>> res;
            vector<int> t;
            sort(candidates.begin(), candidates.end());
            help(candidates, target, 0, t, res);
            return res;
        }
    };
    ```