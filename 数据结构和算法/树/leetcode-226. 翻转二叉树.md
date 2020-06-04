- [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4572877.html)
- [官方解法](https://leetcode-cn.com/problems/invert-binary-tree/solution/fan-zhuan-er-cha-shu-by-leetcode/)
- 解法一：递归
    + 先序遍历每一个节点，然后进行交换左右节点的顺序即可。
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
        TreeNode* invertTree(TreeNode* root) {
            if(!root)
                return NULL;
            TreeNode* t = root->left;
            root->left = invertTree(root->right);
            root->right = invertTree(t);
            return root;
        }
    };
    ```

- 解法二：迭代
    + 非递归的方法也不复杂，跟二叉树的层序遍历一样，需要用queue来辅助，先把根节点排入队列中，然后从队中取出来，交换其左右节点，如果存在则分别将左右节点在排入队列中，以此类推直到队列中木有节点了停止循环，返回root即可。代码如下：
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
        TreeNode* invertTree(TreeNode* root) {
            if(!root)
                return NULL;
            queue<TreeNode*> q;
            q.push(root);
            while(!q.empty())
            {
                TreeNode* t = q.front();
                q.pop();
                TreeNode* tmp = t->right;
                t->right = t->left;
                t->left = tmp;
                if(t->left)
                    q.push(t->left);
                if(t->right)
                    q.push(t->right);
            }
            return root;
        }
    };
    ```