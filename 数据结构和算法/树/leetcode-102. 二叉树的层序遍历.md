- [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4051321.html)
- [官方解法](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/er-cha-shu-de-ceng-ci-bian-li-by-leetcode/)
- 解法一：迭代的BFS
    + 建立一个 queue，然后先把根节点放进去，这时候找根节点的左右两个子节点，这时候去掉根节点，此时 queue 里的元素就是下一层的所有节点，**用一个 for 循环遍历它们(或者while)，因为这里知道具体需要遍历多少元素，所以可以用for循环**，然后存到一个一维向量里，遍历完之后再把这个一维向量存到二维向量里，以此类推，可以完成层序遍历，参见代码如下：
    + 我写的代码
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
        vector<vector<int>> levelOrder(TreeNode* root) {
            if(!root)
                return {};
            vector<vector<int>> res;
            vector<int> t;
            queue<TreeNode*> q;
            q.push(root);
            while(!q.empty())
            {
                int len = q.size();//记录每一层需要遍历的元素个数
                while(len > 0)
                {
                    TreeNode* tmp = q.front();
                    q.pop();
                    t.push_back(tmp->val);
                    if(tmp->left)
                        q.push(tmp->left);
                    if(tmp->right)
                        q.push(tmp->right);
                    --len;
                }
                res.push_back(t);
                t.clear();
            }
            return res;
        }
    };
    ```
    + 参考的代码
    ```C++
    class Solution {
    public:
        vector<vector<int>> levelOrder(TreeNode* root) {
            if (!root) return {};
            vector<vector<int>> res;
            queue<TreeNode*> q{{root}};
            while (!q.empty()) {
                vector<int> oneLevel;
                for (int i = q.size(); i > 0; --i) {
                    TreeNode *t = q.front(); q.pop();
                    oneLevel.push_back(t->val);
                    if (t->left) q.push(t->left);
                    if (t->right) q.push(t->right);
                }
                res.push_back(oneLevel);
            }
            return res;
        }
    };
    ```

- 解法二：递归
    + 下面来看递归的写法，核心就在于需要一个二维数组，和一个变量 level。
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
        vector<vector<int>> levelOrder(TreeNode* root) {
            if(!root)
                return {};
            vector<vector<int>> res;
            cursion(root, 0, res);
            return res;
        }
        void cursion(TreeNode* root, int level, vector<vector<int>>& res)
        {
            if(!root)
                return;
            if(level == res.size())//关键 这里很重要
                res.push_back({});
            res[level].push_back(root->val);
            if(root->left)
                cursion(root->left, level + 1, res);
            if(root->right)
                cursion(root->right, level + 1, res);
        }
    };
    ```