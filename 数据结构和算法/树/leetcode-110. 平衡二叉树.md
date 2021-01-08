- [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4045660.html)
- [官方解法](https://leetcode-cn.com/problems/balanced-binary-tree/solution/ping-heng-er-cha-shu-by-leetcode/)
- 解法一：递归
    + 求二叉树是否平衡，根据题目中的定义，高度平衡二叉树是每一个结点的两个子树的深度差不能超过1，那么我们肯定需要一个求各个点深度的函数，然后对每个节点的两个子树来比较深度差，时间复杂度为O(NlgN)，代码如下：
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
        int getLen(TreeNode*node)
        {
            if(!node)
                return 0;
            return max(getLen(node->left) + 1, getLen(node->right) + 1);
        }
        bool isBalanced(TreeNode* root) {
            if(!root)
                return true;
            if(abs(getLen(root->left) - getLen(root->right)) > 1)
                return false;
            return isBalanced(root->left) && isBalanced(root->right);
        }
    };
    ```

- 上面那个方法正确但不是很高效，因为每一个点都会被上面的点计算深度时访问一次，我们可以进行优化。方法是如果我们发现子树不平衡，则不计算具体的深度，而是直接返回-1。那么优化后的方法为：对于每一个节点，我们通过checkDepth方法递归获得左右子树的深度，如果子树是平衡的，则返回真实的深度，若不平衡，直接返回-1，此方法时间复杂度O(N)，空间复杂度O(H)，参见代码如下：
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
        int checkDepth(TreeNode* node)
        {
            if(!node)
                return 0;
            int l = checkDepth(node->left);
            if(l == -1)
                return -1;
            int r = checkDepth(node->right);
            if(r == -1)
                return -1;
            if(abs(l-r) >= 2)
                return -1;
            else return max(l, r) + 1;
        }
        bool isBalanced(TreeNode* root) {
            if(checkDepth(root) == -1)
                return false;
            else
                return true;
        }
    };
```