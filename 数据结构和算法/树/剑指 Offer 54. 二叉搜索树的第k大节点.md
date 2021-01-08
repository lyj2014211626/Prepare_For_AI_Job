- [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
- [参考解法](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/solution/mian-shi-ti-54-er-cha-sou-suo-shu-de-di-k-da-jie-d/)
- 解法一：中序遍历
    + 传统的中序遍历是先左子树，再右子树。这样得到的递增序列。由于题目是求解最大k,因此，这里将中序遍历改成先右子树，再左子树。
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
        void help(TreeNode*node, int &res, int& num, int k)
        {
            if(!node)
                return;
            help(node->right, res, num, k);
            ++num;
            if(num == k)
                res = node->val;
            help(node->left, res, num, k);
        }
        int kthLargest(TreeNode* root, int k) {
            int res = 0;
            int ind = 0;
            help(root, res, ind, k);
            return res;
        }
    };
    ```