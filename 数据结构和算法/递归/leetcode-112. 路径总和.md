- [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
- [参考博客](https://github.com/grandyang/leetcode/issues/112)
- [官方解法](https://leetcode-cn.com/problems/path-sum/solution/lu-jing-zong-he-by-leetcode/)
- 解法一：递归
    + 这里我受111题从根结点叶子节点递归的思想，写入下一段的代码
    + C++
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
        bool hasPathSum(TreeNode* root, int sum) {
            if(!root)   //边界条件的判断 如 [] 0 返回的结果是false
                return false;
            return cur(root, sum);
        }
        bool cur(TreeNode *root, int sum)
        {
            if(root == NULL)
            {
                if(sum == 0)
                    return true;
                else
                    return false;
            }
            if(root->left == NULL)
            {
                return cur(root->right, sum - root->val);
            }
            if (root->right == NULL)
            {
                return cur(root->left, sum - root->val);
            }
            return cur(root->left, sum - root->val) || cur(root->right, sum - root->val);
        }
    };
    ```

    + 但是根据叶子节点的左右节点为空的特性，可以进行简化
    + 这道题给了一棵二叉树，问是否存在一条从跟结点到叶结点到路径，使得经过到结点值之和为一个给定的 sum 值，这里需要用深度优先算法 DFS 的思想来遍历每一条完整的路径，也就是利用递归不停找子结点的左右子结点，而调用递归函数的参数只有当前结点和 sum 值。首先，如果输入的是一个空结点，则直接返回 false，如果如果输入的只有一个根结点，则比较当前根结点的值和参数 sum 值是否相同，若相同，返回 true，否则 false。 这个条件也是递归的终止条件。下面就要开始递归了，由于函数的返回值是 Ture/False，可以同时两个方向一起递归，中间用或 || 连接，只要有一个是 True，整个结果就是 True。递归左右结点时，这时候的 sum 值应该是原 sum 值减去当前结点的值，参见代码如下：
    + C++
    ```
    class Solution {
    public:
        bool hasPathSum(TreeNode* root, int sum) {
            if (!root) return false;
            if (!root->left && !root->right && root->val == sum ) return true;
            return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);//叶子节点和非叶子节点进行遍历 这是在叶子节点进行判断 然后输出结果
        }
    };
    ```

- 解法二：迭代
    + 我们也可以使用迭代的写法，这里用的也是先序遍历的迭代写法，先序遍历二叉树，左右子结点都需要加上其父结点值，这样当遍历到叶结点时，如果和 sum 相等了，那么就说明一定有一条从 root 过来的路径。注意这里不必一定要先处理右子结点，调换下顺序也是可以的，因为不论是先序遍历的根-左-右，还是根-右-左，并不会影响到找路径，参见代码如下：
    + C++
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
        bool hasPathSum(TreeNode* root, int sum) {
            if(!root)
                return false;
            stack<TreeNode *>s;
            s.push(root);
            while(!s.empty())
            {
                auto t = s.top();
                s.pop();
                if(!t->left && !t->right && t->val == sum)
                {
                    return true;
                }
                if(t->right)//这里注意先将右节点压栈
                {
                    t->right->val += t->val;
                    s.push(t->right);
                }
                if(t->left)//再将左节点压栈
                {
                    t->left->val += t->val;
                    s.push(t->left);
                }
            }
            return false;
        }
    };
    ```