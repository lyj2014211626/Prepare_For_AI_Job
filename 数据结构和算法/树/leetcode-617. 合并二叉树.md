- [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
- [参考解法](https://www.cnblogs.com/grandyang/p/7058935.html)
- [官方解法](https://leetcode-cn.com/problems/merge-two-binary-trees/solution/he-bing-er-cha-shu-by-leetcode/)
- 递归
    + 这道题给了两个二叉树，让我们合并成一个，规则是，都存在的结点，就将结点值加起来，否则空的位置就由另一个树的结点来代替。那么根据过往经验，处理二叉树问题的神器就是递归。根据题目中的规则，如果要处理的相同位置上的两个结点都不存在的话，直接返回即可，如果 t1 存在，t2 不存在，就以 t1 的结点值建立一个新结点，然后分别对 t1 的左右子结点和空结点调用递归函数，反之，如果 t1 不存在，t2 存在，就以 t2 的结点值建立一个新结点，然后分别对 t2 的左右子结点和空结点调用递归函数。如果 t1 和 t2 都存在，就以 t1 和 t2 的结点值之和建立一个新结点，然后分别对 t1 的左右子结点和 t2 的左右子结点调用递归函数，参见代码如下：
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
        TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
            if(!t1)
                return t2;
            if(!t2)
                return t1;
            TreeNode* head = create(t1, t2);
            return head;

        }
        TreeNode* create(TreeNode* t1, TreeNode* t2)
        {
            if(!t1)
            {
                return t2;
            }
            if(!t2)
            {
                return t1;
            }
            TreeNode* head = new TreeNode(t1->val + t2->val);
            head->left = create(t1->left, t2->left);
            head->right = create(t1->right, t2->right);
            return head;
        }
    };
    ```

- 简化版本
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
        TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
            if(!t1)
                return t2;
            if(!t2)
                return t1;
            t1->val = t1->val + t2->val;
            t1->left = mergeTrees(t1->left, t2->left);
            t1->right = mergeTrees(t1->right, t2->right);
            return t1;

        }
    };
    ```