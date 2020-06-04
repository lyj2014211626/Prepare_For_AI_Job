- [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
- [参考博客](https://github.com/grandyang/leetcode/issues/98)
- [官方解法](https://leetcode-cn.com/problems/validate-binary-search-tree/solution/yan-zheng-er-cha-sou-suo-shu-by-leetcode/)
- 解法一：递归
    + 乍一看，这是一个平凡的问题。只需要遍历整棵树，检查 node.right.val > node.val 和 node.left.val < node.val 对每个结点是否成立。![Alt text](https://pic.leetcode-cn.com/cfdd62ec84e98f9ea88f885f617b692222c5ec144d228b1838150e18c60be0c2-image.png)
    + 问题是，这种方法并不总是正确。不仅右子结点要大于该节点，整个右子树的元素都应该大于该节点。例如:![Alt text](https://pic.leetcode-cn.com/53f4c7ce658d9611677d12940b06373ae3c900d5c8b94ac3bbf587e152efdba5-image.png)
    + **这意味着我们需要在遍历树的同时保留结点的上界与下界**，**在比较时不仅比较子结点的值，也要与上下界比较**。
    + C++
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
        bool isValidBST(TreeNode* root) {
            return cursion(root, LONG_MIN, LONG_MAX);
        }
        bool cursion(TreeNode *node, long lower, long upper)
        {
            if(!node)
                return true;
            if(node->val <= lower || node->val >= upper)//这里是判断错误的
                return false;
            return cursion(node->left, lower, node->val) && cursion(node->right, node->val, upper);//每次遍历左右子树 都会更新上下边界
        }
    };
    ```

- 解法二：中序遍历 - 递归
    + 这题实际上简化了难度，因为有的时候题目中的二叉搜索树会定义为左<=根<右，而这道题设定为一般情况左<根<右，那么就可以用中序遍历来做。因为如果不去掉左=根这个条件的话，那么下边两个数用中序遍历无法区分：
```
       20       20
       /          \
     20           20
```
    + 它们的中序遍历结果都一样，但是左边的是 BST，右边的不是 BST。去掉等号的条件则相当于去掉了这种限制条件。下面来看使用中序遍历来做，这种方法思路很直接，通过中序遍历将所有的节点值存到一个数组里，然后再来判断这个数组是不是有序的，代码如下：
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
        bool isValidBST(TreeNode* root) {
            vector<int> res;
            inorder(root, res);
            for(int i = 1; i < res.size(); ++i)
            {
                if(res[i] <= res[i - 1])
                    return false;
            }
            return true;
        }
        void inorder(TreeNode *node, vector<int> &res)
        {
            if(!node)
                return;
            inorder(node->left, res);
            res.push_back(node->val);
            inorder(node->right, res);
        }
    };
    ```

- 解法三：中序遍历了 - 非递归
    + 中序遍历的非递归版本，用栈保存之前访问的节点
    ```C++
    class Solution {
    public:
        bool isValidBST(TreeNode* root) {
            stack<TreeNode*> s;
            TreeNode *p = root, *pre = NULL;//开始初始化时空
            while (p || !s.empty()) {
                while (p) {
                    s.push(p);
                    p = p->left;
                }
                p = s.top(); s.pop();
                if (pre && p->val <= pre->val) return false;//前序节点不空
                pre = p;
                p = p->right;
            }
            return true;
        }
    };
    ```

- 解法四：递归
    + 就是中序遍历的递归形式，只不过不保存遍历结果，直接进行比较判断。
    ```C++
    class Solution {
    public:
        bool isValidBST(TreeNode* root) {
            TreeNode *pre = NULL;
            return inorder(root, pre);
        }
        bool inorder(TreeNode* node, TreeNode*& pre) {
            if (!node) return true;
            bool res = inorder(node->left, pre);
            if (!res) return false;
            if (pre) {
                if (node->val <= pre->val) return false;
            }
            pre = node;
            return inorder(node->right, pre);
        }
    };
    ```

- 解法五：线索二叉树
    + 最后还有一种方法，由于中序遍历还有非递归且无栈的实现方法，称之为 Morris 遍历，可以参考博主之前的博客 Binary Tree Inorder Traversal，这种实现方法虽然写起来比递归版本要复杂的多，但是好处在于是 O(1) 空间复杂度，参见代码如下：
    ```C++
    class Solution {
    public:
        bool isValidBST(TreeNode *root) {
            if (!root) return true;
            TreeNode *cur = root, *pre, *parent = NULL;
            bool res = true;
            while (cur) {
                if (!cur->left) {
                    if (parent && parent->val >= cur->val) res = false;
                    parent = cur;
                    cur = cur->right;
                } else {
                    pre = cur->left;
                    while (pre->right && pre->right != cur) pre = pre->right;
                    if (!pre->right) {
                        pre->right = cur;
                        cur = cur->left;
                    } else {
                        pre->right = NULL;
                        if (parent->val >= cur->val) res = false;
                        parent = cur;
                        cur = cur->right;
                    }
                }
            }
            return res;
        }
    };
    ```