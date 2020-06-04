- [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4293853.html)
- [官方解法](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--26/)
- 解法一：递归
    + 这道题要求把二叉树展开成链表，根据展开后形成的链表的顺序分析出是使用**先序遍历**，那么只要是数的遍历就有递归和非递归的两种方法来求解，这里我们也用两种方法来求解。首先来看递归版本的，思路是先利用 DFS 的思路找到最左子节点，然后再进入到右子树进行递归，最后回到其父节点，把其父节点和右子节点断开，将原左子结点连上父节点的右子节点上，然后再把原右子节点连到新右子节点的右子节点上，然后再回到上一父节点做相同操作。代码如下：
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
        void flatten(TreeNode* root) {
            if(!root)
                return;
            if(root->left)
                flatten(root->left);
            if(root->right)
                flatten(root->right);
            TreeNode* t = root->right;
            root->right = root->left;
            root->left = NULL;  //记得把左子树置空
            while(root->right)
                root = root->right;
            root->right = t;
        }
    };
    ```

- 解法二：迭代
    + 下面再来看非迭代版本的实现，这个方法是从根节点开始出发，先检测其左子结点是否存在，如存在则将根节点和其右子节点断开，将左子结点及其后面所有结构一起连到原右子节点的位置，把原右子节点连到元左子结点最后面的右子节点之后。代码如下：
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
        void flatten(TreeNode* root) {
            TreeNode* cur = root;
            while(cur)
            {
                if(cur->left)
                {
                    TreeNode* p = cur->left;
                    while(p->right)
                        p = p ->right;
                    p->right = cur->right;
                    cur->right = cur->left;
                    cur->left = NULL;
                }
                cur = cur->right;
            }
        }
    };
    ```

- 解法三：前序迭代解法
    + 此题还可以延伸到用中序，后序，层序的遍历顺序来展开原二叉树，分别又有其对应的递归和非递归的方法，有兴趣的童鞋可以自行实现。
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
        void flatten(TreeNode* root) {
            if(!root)
                return;
            stack<TreeNode*> s;
            s.push(root);
            while(!s.empty())
            {
                TreeNode* t = s.top();
                s.pop();
                if(t->left)
                {
                    TreeNode* p = t->left;
                    while(p->right)
                        p = p->right;
                    p->right = t->right;
                    t->right = t->left;
                    t->left = NULL;
                }
                if(t->right)
                    s.push(t->right);
            }
        }
    };
    ```