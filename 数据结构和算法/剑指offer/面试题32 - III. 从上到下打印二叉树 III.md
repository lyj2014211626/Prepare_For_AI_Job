- [面试题32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)
- 解法一：用双端队列进行操作
    + **奇偶层的打印顺序不一样是相反的，可以利用层数偶数与否调用reverse来解决，但是海量数据时这个效率很低，不推荐。**
    + 奇数层，从头到尾访问队列，左子树到右子树的顺序 以push_back的方式进队列
    + 偶数层，从尾到头访问队列，右子树到左子树的顺序 以push_front的方式进队列
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
            deque<TreeNode*> dq;
            dq.push_back(root);
            int level = 1;
            while(!dq.empty())
            {
                int len = dq.size();
                vector<int> vec;
                if(level % 2)
                {
                    while(len > 0)
                    {
                        TreeNode* t = dq.front();
                        dq.pop_front();
                        vec.push_back(t->val);
                        if(t->left)
                            dq.push_back(t->left);
                        if(t->right)
                            dq.push_back(t->right);
                        --len;
                    }
                }
                else
                {
                    while(len > 0)
                    {
                        TreeNode* t = dq.back();
                        dq.pop_back();
                        vec.push_back(t->val);
                        if(t->right)
                            dq.push_front(t->right);
                        if(t->left)
                            dq.push_front(t->left);
                        --len;
                    }
                }
                res.push_back(vec);
                ++level;
            }
            return res;
        }
    };
    ```