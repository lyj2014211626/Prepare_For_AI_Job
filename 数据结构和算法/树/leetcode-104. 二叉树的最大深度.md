- [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4051348.html)
- [官方解法](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-by-leetcode/)
- 解法一：递归
    + 递归解法，求出最大的深度。
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
        int maxDepth(TreeNode* root) {
            if(!root)
                return 0;
            return max(maxDepth(root->left) + 1, maxDepth(root->right)+ 1);
        }
    };
    ```

- 解法二：层次遍历
    + 