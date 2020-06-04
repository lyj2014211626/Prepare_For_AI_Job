- [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [参考博客](https://github.com/grandyang/leetcode/issues/21)
- [官方解法](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/he-bing-liang-ge-you-xu-lian-biao-by-leetcode/)
- 解法一：就普通的遍历法
    + 这道混合插入有序链表和我之前那篇混合插入有序数组非常的相似 Merge Sorted Array，仅仅是数据结构由数组换成了链表而已，代码写起来反而更简洁。具体思想就是新建一个链表，然后比较两个链表中的元素值，把较小的那个链到新链表中，由于两个输入链表的长度可能不同，所以最终会有一个链表先完成插入所有元素，则直接另一个未完成的链表直接链入新链表的末尾。代码如下：
    + C++版本
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
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            ListNode *head = new ListNode(-1);
            ListNode *cur = head;
            while(l1 && l2)
            {
                if(l1->val <= l2->val)
                {
                    cur->next = l1;
                    l1 = l1 ->next;
                }
                else
                {
                    cur->next = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            }
            cur->next = l1 ? l1 : l2;
            return head->next;
        }
    };
    ```

- 解法二：递归
    + 下面我们来看递归的写法，当某个链表为空了，就返回另一个。然后核心还是比较当前两个节点值大小，如果 l1 的小，那么对于 l1 的下一个节点和 l2 调用递归函数，将返回值赋值给 l1.next，然后返回 l1；否则就对于 l2 的下一个节点和 l1 调用递归函数，将返回值赋值给 l2.next，然后返回 l2，参见代码如下：
    + C++版本
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
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            if(!l1) //l1为空，则返回l2
                return l2;
            if(!l2)
                return l1;
            if(l1->val < l2->val)
            {
                l1->next = mergeTwoLists(l1->next, l2);//继续调用l1的下一个元素和l2的当前元素
                return l1; //返回l1,不是l1->next，因为本次函数的调用就是l1，相等于直接返回本轮的结果
            }
            else
            {
                l2->next = mergeTwoLists(l1, l2->next);
                return l2;
            }
        }
    };
    ```

    + 下面这种递归的写法去掉了 if 从句，看起来更加简洁一些，但是思路并没有什么不同：
    ```
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            if (!l1) return l2;
            if (!l2) return l1;
            ListNode *head = l1->val < l2->val ? l1 : l2;
            ListNode *nonhead = l1->val < l2->val ? l2 : l1;
            head->next = mergeTwoLists(head->next, nonhead);
            return head;
        }
    };
    ```