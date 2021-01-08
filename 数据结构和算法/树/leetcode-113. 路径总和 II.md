- [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)
- [参考博客](https://github.com/grandyang/leetcode/issues/113)
- 解法一：递归
    + 这道二叉树路径之和在之前那道题 Path Sum 的基础上又需要找出路径，但是基本思想都一样，还是需要用深度优先搜索 DFS，只不过数据结构相对复杂一点，需要用到二维的 vector，而且每当 DFS 搜索到新结点时，都要保存该结点。而且每当找出一条路径之后，都将这个保存为一维 vector 的路径保存到最终结果二维 vector 中。并且，每当 DFS 搜索到子结点，发现不是路径和时，返回上一个结点时，需要把该结点从一维 vector 中移除，参见代码如下：
    + 自己的版本
    ```C++
    /**
     * Definition for a binary tree node.
     * struct TreeNode {
     *     int val;
     *     TreeNode *left;
     *     TreeNode *right;
     *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
     * };
     */
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int sum) {
            if(!root)
                return{};
            vector<vector<int>> res;
            vector<int> vec;
            cursion(root, sum, vec, res);
            return res;
        }
        void cursion(TreeNode* node, int sum, vector<int>& vec, vector<vector<int>>& res)
        {
            if(!node ->left && !node->right)
            {
                if(node->val == sum)
                {
                    vec.push_back(node->val);
                    res.push_back(vec);
                    vec.pop_back();
                }
                return;
            }
            if(node->left)
            {
                vec.push_back(node->val);
                cursion(node->left, sum - node->val, vec, res);
                vec.pop_back();
            }
            if(node->right)
            {
                vec.push_back(node->val);
                cursion(node->right, sum - node->val, vec, res);
                vec.pop_back();
            }
        }
    };
    ```
    + C++ 另外一个版本 注意二个版本之间的差异
    
    ```
    /**
     * Definition for a binary tree node.
     * struct TreeNode {
     *     int val;
     *     TreeNode *left;
     *     TreeNode *right;
     *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
     * };
     */
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int sum) {
            vector<vector<int>> res;
            vector<int> t;
            cursion(root, sum, t, res);
            return res;
        }
        void cursion(TreeNode *node, int sum, vector<int>& t, vector<vector<int>>& res)
        {
            if(!node)
                return ;
            t.push_back(node->val);
            if(!node->left && !node->right && node->val == sum)
            {
                res.push_back(t);
                // return; 这里的return 一定不能写 否则结果不对 自己想想 而且是为了执行后面的回溯 否则代码不对
            }

            cursion(node->left, sum - node->val, t, res);

            cursion(node->right, sum - node->val, t, res);
            t.pop_back();//重要 回溯  进行弹出
        }
    };
    ```

- 第三个版本
    ```C++
    /**
     * Definition for a binary tree node.
     * struct TreeNode {
     *     int val;
     *     TreeNode *left;
     *     TreeNode *right;
     *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
     * };
     */
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int sum) {
            if(!root)
                return {};
            vector<vector<int>> res;
            vector<int> vec;
            cursion(root, sum, vec, res);
            return res;
        }
        void cursion(TreeNode* root, int sum, vector<int>& vec, vector<vector<int>>& res)
        {
            if(!root)
                return;
            vec.push_back(root->val);
            sum-= root->val;
            if(!root->left  && !root->right && sum == 0)
            {
                res.push_back(vec);
            }
            cursion(root->left, sum, vec, res);
            cursion(root->right, sum, vec, res);
            vec.pop_back();
        }
    };
    ```
- 解法二：迭代
    + 下面这种方法是迭代的写法，用的是中序遍历的顺序，参考之前那道 Binary Tree Inorder Traversal，中序遍历本来是要用栈来辅助运算的，由于要取出路径上的结点值，所以用一个 vector 来代替 stack，首先利用 while 循环找到最左子结点，在找的过程中，把路径中的结点值都加起来，这时候取出 vector 中的尾元素，如果其左右子结点都不存在且当前累加值正好等于 sum 了，将这条路径取出来存入结果 res 中，下面的部分是和一般的迭代中序写法有所不同的地方，由于中序遍历的特点，遍历到当前结点的时候，是有两种情况的，有可能此时是从左子结点跳回来的，此时正要去右子结点，则当前的结点值还是算在路径中的；也有可能当前是从右子结点跳回来的，并且此时要跳回上一个结点去，此时就要减去当前结点值，因为其已经不属于路径中的结点了。为了区分这两种情况，这里使用一个额外指针 pre 来指向前一个结点，如果右子结点存在且不等于 pre，直接将指针移到右子结点，反之更新 pre 为 cur，cur 重置为空，val 减去当前结点，st 删掉最后一个结点，参见代码如下：
    + C++
    ```
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int sum) {
            vector<vector<int>> res;
            vector<TreeNode*> st;
            TreeNode *cur = root, *pre = nullptr;
            int val = 0;
            while (cur || !st.empty()) {
                while (cur) {
                    st.push_back(cur);
                    val += cur->val;
                    cur = cur->left;
                }
                cur = st.back(); 
                if (!cur->left && !cur->right && val == sum) {
                    vector<int> v;
                    for (auto &a : st) v.push_back(a->val);
                    res.push_back(v);
                }
                if (cur->right && cur->right != pre) {
                    cur = cur->right;
                } else {
                    pre = cur;
                    val -= cur->val;
                    st.pop_back();
                    cur = nullptr;
                }
            }
            return res;
        }
    };
    ```