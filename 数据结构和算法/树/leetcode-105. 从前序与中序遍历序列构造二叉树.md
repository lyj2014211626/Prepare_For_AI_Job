- [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- [参考博客](https://github.com/grandyang/leetcode/issues/105)
- [官方解答](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/cong-qian-xu-he-zhong-xu-bian-li-xu-lie-gou-zao-er/)
- 解法一：递归
    + 这道题要求用先序和中序遍历来建立二叉树，跟之前那道 Construct Binary Tree from Inorder and Postorder Traversal 原理基本相同，针对这道题，由于先序的顺序的第一个肯定是根，所以原二叉树的根节点可以知道，题目中给了一个很关键的条件就是树中没有相同元素，有了这个条件就可以在中序遍历中也定位出根节点的位置，并以根节点的位置将中序遍历拆分为左右两个部分，分别对其递归调用原函数，参见代码如下：
    + **对于二叉树类型题目的理解就是只关心对当前节点的一系列操作，左右孩子只需要传递当前节点左右孩子指针即可，递归调用的理解又加深了。**
    + C++代码
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
        TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
            return cursion(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
        }
        TreeNode* cursion(vector<int>& preorder,int pre_l, int pre_r, vector<int>& inorder, int in_l, int in_r)
        {
            if(pre_l > pre_r || in_l > in_r)//这里返回空指针很关键
                return NULL;
            else
            {
                int k = in_l;
                for(; k <= in_r; ++k)
                {
                    if(preorder[pre_l] == inorder[k])
                        break;
                }
                TreeNode *node = new TreeNode(preorder[pre_l]);
                node->left = cursion(preorder, pre_l + 1, pre_l + k - in_l, inorder, in_l, k - 1);//各个数组的索引的改变很关键 很容易错 这里就是递归的进行左右子树的调用
                node->right = cursion(preorder, pre_l + k - in_l + 1, pre_r, inorder, k + 1, in_r);
                return node;
            }
        }
    };
    ```