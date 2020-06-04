- [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4296193.html)
- [官方解法](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solution/cong-zhong-xu-yu-hou-xu-bian-li-xu-lie-gou-zao-e-5/)
- 解法一：递归
    + 这道题要求从中序和后序遍历的结果来重建原二叉树，我们知道中序的遍历顺序是左-根-右，后序的顺序是左-右-根，对于这种树的重建一般都是采用递归来做。**由于后序的顺序的最后一个肯定是根**，所以原二叉树的根节点可以知道，题目中给了一个很关键的条件就是树中没有相同元素，有了这个条件我们就可以在中序遍历中也定位出根节点的位置，并以根节点的位置将中序遍历拆分为左右两个部分，分别对其递归调用原函数。代码如下：
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
        TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
            return cursion(inorder, 0, inorder.size() - 1, postorder, 0, postorder.size() - 1);
        }
        TreeNode* cursion(vector<int>& inorder, int in_l, int in_r, vector<int>& postorder, int ps_l, int ps_r)
        {
            if(in_l > in_r || ps_l > ps_r)
            {
                return NULL;
            }
            else
            {
                int index = in_l;
                for(; index <= in_r; ++index)
                {
                    if(inorder[index] == postorder[ps_r])
                        break;
                }
                TreeNode* node = new TreeNode(postorder[ps_r]);
                node->left = cursion(inorder, in_l, index - 1, postorder, ps_l, ps_l + index - in_l - 1);//这里容易错
                node->right = cursion(inorder, index + 1, in_r, postorder, ps_l + index - in_l, ps_r - 1);
                return node;
            }
        }
    };
    ```
    + 上述代码中需要小心的地方就是递归是postorder的左右index很容易写错，**比如 pLeft + i - iLeft - 1**, 这个又长又不好记，首先我们要记住 i - iLeft 是计算inorder中根节点位置和左边起始点的距离，然后再加上postorder左边起始点然后再减1。我们可以这样分析，如果根节点就是左边起始点的话，那么拆分的话左边序列应该为空集，此时i - iLeft 为0， pLeft + 0 - 1 < pLeft, 那么再递归调用时就会返回NULL, 成立。如果根节点是左边起始点紧跟的一个，那么i - iLeft 为1， pLeft + 1 - 1 = pLeft，再递归调用时还会生成一个节点，就是pLeft位置上的节点，为原二叉树的一个叶节点。