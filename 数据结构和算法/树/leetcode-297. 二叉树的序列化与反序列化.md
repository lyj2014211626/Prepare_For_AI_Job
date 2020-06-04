- [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4913869.html)
- 解法一：层次遍历
    + 层序遍历的非递归解法，这种方法略微复杂一些，我们需要借助queue来做，本质是BFS算法，也不是很难理解，就是BFS算法的常规套路稍作修改即可，参见代码如下：
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
    class Codec {
    public:

        // Encodes a tree to a single string.
        string serialize(TreeNode* root) {
            ostringstream out;
            queue<TreeNode*> q;
            if(root)
                q.push(root);
            while(!q.empty())
            {
                TreeNode* t = q.front();
                q.pop();
                if(t)
                {
                    out<<t->val<<' ';
                    q.push(t->left);
                    q.push(t->right);
                }
                else
                {
                    out<<"# ";
                }
            }
            return out.str();
        }

        // Decodes your encoded data to tree.
        TreeNode* deserialize(string data) {
            if(data.empty())
                return nullptr;
            istringstream in(data);
            queue<TreeNode*> q;
            string val;
            in>>val;
            TreeNode* res = new TreeNode(stoi(val));
            TreeNode* cur = res;
            q.push(cur);
            while(!q.empty())
            {
                TreeNode* t = q.front();
                q.pop();
                if(!(in>>val))
                    break;
                if(val != "#")
                {
                    cur = new TreeNode(stoi(val));
                    q.push(cur);
                    t->left = cur;
                    
                }
                if(!(in>>val))
                    break;
                if(val != "#")
                {
                    cur = new TreeNode(stoi(val));
                    q.push(cur);
                    t->right = cur;
                }
            }
            return res;
        }
    };

    // Your Codec object will be instantiated and called as such:
    // Codec codec;
    // codec.deserialize(codec.serialize(root));
    ```

- 解法二：先序遍历 + 递归
    + 这题有两种解法，分别为先序遍历的递归解法和层序遍历的非递归解法。先来看先序遍历的递归解法，非常的简单易懂，我们需要接入输入和输出字符串流istringstream和ostringstream，对于序列化，我们从根节点开始，如果节点存在，则将值存入输出字符串流，然后分别对其左右子节点递归调用序列化函数即可。对于去序列化，我们先读入第一个字符，以此生成一个根节点，然后再对根节点的左右子节点递归调用去序列化函数即可，参见代码如下
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
    class Codec {
    public:

        // Encodes a tree to a single string.
        string serialize(TreeNode* root) {
            ostringstream out;
            serialize(root, out);
            return out.str();
        }

        // Decodes your encoded data to tree.
        TreeNode* deserialize(string data) {
            istringstream in(data);
            TreeNode* root = deserialize(in);
            return root;
        }
    private:
        void serialize(TreeNode* root, ostringstream& out)
        {
            if(root)
            {
                out<<root->val<<' ';
                serialize(root->left, out);
                serialize(root->right, out);
            }
            else
            {
                out<<"# ";
            }
        }
        TreeNode* deserialize(istringstream& in)
        {
            string val;
            in>>val;
            if(val == "#")
                return nullptr;
            TreeNode* t = new TreeNode(stoi(val));
            t->left = deserialize(in);
            t->right = deserialize(in);
            return t;
        }
    };

    // Your Codec object will be instantiated and called as such:
    // Codec codec;
    // codec.deserialize(codec.serialize(root));
    ```