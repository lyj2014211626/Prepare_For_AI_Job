- [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
- [参考博客](https://github.com/grandyang/leetcode/issues/24)
- [官方解答](https://leetcode-cn.com/problems/swap-nodes-in-pairs/solution/hua-jie-suan-fa-24-liang-liang-jiao-huan-lian-biao/)
- 解法一：非递归
    + 对于迭代实现，还是需要建立 dummy 节点，注意在连接节点的时候，最好画个图，以免把自己搞晕了，参见代码如下：
    + **至于什么地方加哨兵节点 什么时候不加这个问题就是看头节点是否发生了变化，这里头节点在计算的过程中发生了变化 其实很多链表的问题在加上哨兵节点后会**
    + C++
    ```
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
        ListNode* swapPairs(ListNode* head) {
            ListNode *dummy = new ListNode(-1);
            dummy->next = head;
            ListNode *cur = dummy;
            while(cur->next != NULL && cur->next->next != NULL)
            {
                ListNode *end = cur->next->next;
                cur->next->next = end->next;
                end->next = cur->next;
                cur->next = end;
                cur = cur->next->next;
            }
            return dummy->next;
        }
    };
    ```

- 解法二：递归实现
    + 这题的递归形式真是太妙了，还是那句话，神理解递归，人理解迭代。写递归还是那句话，**理解当前步骤即可，上下调用的步骤就直接考虑返回值即可**。代码如下：
    + C++
    ```
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
        ListNode* swapPairs(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode * t = head->next;//当前节点的指针的下一个节点指针
            head->next = swapPairs(head->next->next);//当前节点指针指向上一个函数调用的返回
            t->next = head;//中间二个节点的指针反向
            return t;
        }
    };
    ```