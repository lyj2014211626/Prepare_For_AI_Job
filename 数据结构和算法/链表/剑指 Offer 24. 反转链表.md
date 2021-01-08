- [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)
- [参考解法](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/ru-guo-ni-kan-wan-ping-lun-he-ti-jie-huan-you-wen-/)
- 迭代三指针
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
        ListNode* reverseList(ListNode* head) {
            ListNode* pre = NULL;
            ListNode* cur = head;
            ListNode* t = NULL;
            while(cur)
            {
                t = cur->next;
                cur->next = pre;
                pre = cur;
                cur = t;
            }
            return pre;
        }
    };
    ```
- 迭代双指针
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
        ListNode* reverseList(ListNode* head) {
            if(!head)
                return head;
            ListNode* dummy = new ListNode(-1);
            dummy->next = head;
            ListNode* cur = dummy;
            ListNode* last = cur->next;
            while(last->next)
            {
                ListNode* t = last->next;
                last->next = t->next;
                t->next = cur->next;
                cur->next = t;
            }
            return dummy->next;
        }
    };
    ```
- 递归
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
        ListNode* reverseList(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode* cur = reverseList(head->next);
            head->next->next = head;
            head->next = NULL;
            return cur;
        }
    };
    ```