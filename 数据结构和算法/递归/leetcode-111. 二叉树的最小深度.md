- [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
- [参考博客](https://github.com/grandyang/leetcode/issues/111)
- [官方解答](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/er-cha-shu-de-zui-xiao-shen-du-by-leetcode/)
- 解法一：递归
    + 二叉树的经典问题之最小深度问题就是就最短路径的节点个数，还是用深度优先搜索DFS来完成，万能的递归啊。首先判空，若当前结点不存在，直接返回0。然后看若左子结点不存在，那么对右子结点调用递归函数，并加1返回。反之，若右子结点不存在，那么对左子结点调用递归函数，并加1返回。若左右子结点都存在，则分别对左右子结点调用递归函数，将二者中的较小值加1返回即可，参见代码如下：
    + C++ **这里需要注意的是从根节点到叶子节点的情况，也就是说不是有一个节点为空就返回，这不是叶子接单，和树的最深进行比较**
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
        int minDepth(TreeNode* root) {
            if(!root)
                return 0;
            if(!root->left)//左节点为空 则继续遍历右节点
                return minDepth(root->right) + 1;
            if(!root->right)//右子树为空 则继续遍历左节点
                return minDepth(root->left) + 1;
            //这里是当前节点左右都不为空 然后进行递归遍历
            return min(minDepth(root->left) + 1, minDepth(root->right) + 1);
        }
    };
    ```

- 解法二：层次遍历
    + 我们也可以是迭代来做，层序遍历，记录遍历的层数，一旦我们遍历到第一个叶结点，就将当前层数返回，即为二叉树的最小深度，参见代码如下：
    + C++ 注意这里使用的是队列
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
        int minDepth(TreeNode* root) {
            if(!root)//临界条件的判断
                return 0;
            queue<TreeNode*> q;
            q.push(root);
            int dep = 0;
            while(!q.empty())//队列不为空
            {
                ++dep;
                for(int i = q.size(); i >0; --i)//这里用到一个技巧 就是 i初始化的时候是从 q.size 因为这里是记录了当前层内所有节点 后续队列的大小会改变
                {
                    auto t = q.front();//访问第一个元素
                    q.pop();    //这里记得弹出
                    if(!t->left && !t->right)
                        return dep;
                    if(t->left)
                        q.push(t->left);
                    if(t->right)
                        q.push(t->right);
                }
            }
            return -1;
        }
    };
    ```