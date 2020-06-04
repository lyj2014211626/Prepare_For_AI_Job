- [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4069003.html)
- [官方解法](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/solution/java-ya-jie-dian-fei-di-gui-rong-yi-li-jie-yong-sh/)
- 解法一：迭代
    + 这里可能头节点会出现重复，所以需要新创建哑节点。
    ```C++
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            ListNode* dummy = new ListNode(-1);
            dummy->next = head;
            ListNode* cur = head;
            ListNode* pre = dummy;
            while(cur && cur->next)
            {
                while(cur->next && cur->val == cur->next->val)
                    cur = cur->next;
                if(pre->next == cur)//这里判断的是指针是否相等，而不是值。即是否出现重复。
                    pre = pre->next;
                else
                    pre->next = cur->next;
                cur = cur->next;// 更换当前指针指向
            }
            return dummy->next;
        }
    };
    ```
    + 参考的博客答案
    + 由于链表开头可能会有重复项，被删掉的话头指针会改变，而最终却还需要返回链表的头指针。所以需要**定义一个新的节点**，然后链上原链表，然后定义一个前驱指针和一个现指针，每当前驱指针指向新建的节点，现指针从下一个位置开始往下遍历，遇到相同的则继续往下，直到遇到不同项时，把前驱指针的next指向下面那个不同的元素。如果现指针遍历的第一个元素就不相同，则把前驱指针向下移一位。代码如下：
    ```C++
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            if (!head || !head->next) return head;
            ListNode *dummy = new ListNode(-1), *pre = dummy;
            dummy->next = head;
            while (pre->next) {
                ListNode *cur = pre->next;
                while (cur->next && cur->next->val == cur->val) {
                    cur = cur->next;
                }
                if (cur != pre->next) pre->next = cur->next;
                else pre = pre->next;
            }
            return dummy->next;
        }
    };
    ```
- 递归解法
    + 同样，我们也可以使用递归来做，首先判空，如果 head 为空，直接返回。然后判断，若 head 之后的结点存在，且值相等，那么先进行一个 while 循环，跳过后面所有值相等的结点，到最后一个值相等的点停下。比如对于例子2来说，head 停在第三个结点1处，然后对后面一个结点调用递归函数，即结点2，这样做的好处是，返回的值就完全把所有的结点1都删掉了。若 head 之后的结点值不同，那么还是对 head 之后的结点调用递归函数，将返回值连到 head 的后面，这样 head 结点还是保留下来了，因为值不同嘛，最后返回 head 即可，参见代码如下：
    ```C++
    /**
     * Definition for singly-linked list.
     * struct ListNode {
     *     int val;
     *     ListNode *next;
     *     ListNode(int x) : val(x), next(NULL) {}
     * };
     */
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            if(!head)//为空 直接返回
                return head;
            if(head->next && head->val == head->next->val)//如果当前有重复的元素
            {
                while(head->next && head->val == head->next->val)
                    head = head->next;
                return deleteDuplicates(head->next);
            }
            head->next = deleteDuplicates(head->next);//正成遍历返回
            return head;//返回当前层的结果
        }
    };
        ```