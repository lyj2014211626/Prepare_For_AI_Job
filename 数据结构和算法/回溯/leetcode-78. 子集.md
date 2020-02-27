- [78. 子集](https://leetcode-cn.com/problems/subsets/)
- [参考常用解答](https://github.com/grandyang/leetcode/issues/78)
- [参考2](https://www.cnblogs.com/TenosDoIt/p/3451902.html)
- 解法一：非递归
    + 这道求子集合的问题，由于其要列出所有结果，按照以往的经验，肯定要是要用递归来做。这道题其实它的非递归解法相对来说更简单一点，下面我们先来看非递归的解法，由于题目要求子集合中数字的顺序是非降序排列的，所有我们需要预处理，先给输入数组排序，然后再进一步处理，最开始我在想的时候，是想按照子集的长度由少到多全部写出来，比如子集长度为0的就是空集，空集是任何集合的子集，满足条件，直接加入。下面长度为1的子集，直接一个循环加入所有数字，子集长度为2的话可以用两个循环，但是这种想法到后面就行不通了，因为循环的个数不能无限的增长，所以我们必须换一种思路。我们可以一位一位的网上叠加，比如对于题目中给的例子 [1,2,3] 来说，最开始是空集，那么我们现在要处理1，就在空集上加1，为 [1]，现在我们有两个自己 [] 和 [1]，下面我们来处理2，我们在之前的子集基础上，每个都加个2，可以分别得到 [2]，[1, 2]，那么现在所有的子集合为 [], [1], [2], [1, 2]，同理处理3的情况可得 [3], [1, 3], [2, 3], [1, 2, 3], 再加上之前的子集就是所有的子集合了，代码如下：
    + **这个非递归的节解题思路就是从前开始遍历，每添加一个元素就在前面统计的所有情况里增加一个添加该元素的组合。这种迭代的思路就是用计算机的思维解决问题的关键。这道题的数学思想其实很简单，就是组合问题，但是怎么去统计不同组合的情况，并找出规律用计算机的思维解决问题很重要！！**
    + 整个添加的顺序为：
    + []
    + [1]
    + [2]
    + [1 2]
    + [3]
    + [1 3]
    + [2 3]
    + [1 2 3]
    ```
    class Solution {
    public:
        vector<vector<int>> subsets(vector<int>& nums) {
            vector<vector<int>> res(1);
            //sort(nums.begin(), nums.end());//这一行代码可以不要
            for(int i = 0; i < nums.size(); ++i){
                int size = res.size();
                for(int j = 0; j < size; ++j){
                    res.push_back(res[j]);
                    res.back().push_back(nums[i]);//添加该元素 注意下标是i
                }
            }
            return res;
        }
    };
    ```

- 解法二：回溯法

    ![avatar](https://images0.cnblogs.com/blog/517264/201311/30222556-771b539e28024b35a8cf2537b2f3f134.jpg)

    + 由于原集合每一个数字只有两种状态，要么存在，要么不存在，那么在构造子集时就有选择和不选择两种情况，所以可以构造一棵二叉树，左子树表示选择该层处理的节点，右子树表示不选择，最终的叶节点就是所有子集合，树的结构如下：
    ```
    class Solution {
    public:
        vector<vector<int>> subsets(vector<int>& nums) {
            vector<vector<int>> res;
            vector<int> out;
            cursion(0, nums, out, res);
            return res;
        }
        void cursion(int pos, vector<int>& nums, vector<int>& out, vector<vector<int>>& res){
            if(pos == nums.size()){
                res.push_back(out);
                return ;
            }
            out.push_back(nums[pos]);//选择nums[i]
            cursion(pos + 1, nums, out, res);
            out.pop_back();
            cursion(pos + 1, nums, out, res);//不选择nums[i]
        }
    };
    ```

- 解法三：回溯
    + 写法不一样，有点向我们自己遍历的思路 从第一个元素开始，树的第一层考虑的是一个元素，第二层考虑的是二个元素组合，第三层考虑的是三个元素组合情况。
    + ![avatar](C:\myFile\project\Prepare_For_AI_Job\数据结构和算法\回溯\78.jpg)
    ```
    class Solution {
    public:
        vector<vector<int> > subsets(vector<int> &S) {
            vector<vector<int> > res;
            vector<int> out;
            //sort(S.begin(), S.end());//可以不用
            getSubsets(S, 0, out, res);
            return res;
        }
        void getSubsets(vector<int> &S, int pos, vector<int> &out, vector<vector<int> > &res) {
            res.push_back(out);
            for (int i = pos; i < S.size(); ++i) {//i初始化的位置差别
                out.push_back(S[i]);
                getSubsets(S, i + 1, out, res);
                out.pop_back();
            }
        }
    };
    ```
