- [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4635425.html)
- [官方解法](https://leetcode-cn.com/problems/palindrome-linked-list/solution/)
- 解法一
    + 先找到链表的中间位置，然后将后半个链表进行逆序，最后依次进行比较就行了。
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
        ListNode* reverse(ListNode* pre, ListNode* next)
        {
            ListNode* cur = pre->next;
            ListNode* last = cur->next;
            while(cur->next != next)
            {
                cur->next = last->next;
                last->next = pre->next;
                pre->next = last;
                last = cur->next;
            }
            return pre->next;
        }
        bool isPalindrome(ListNode* head) {
            if(!head || !head->next)
                return true;
            int len = 0;
            ListNode* t = head;
            while(t)
            {
                t = t->next;
                ++len;
            }
            len = (len - 1) / 2;
            ListNode* pre = head;
            ListNode* cur = head;
            while(len > 0)
            {
                cur = cur->next;
                --len;
            }
            cur = reverse(cur, NULL);
            while(pre && cur)
            {
                if(pre->val != cur->val)
                    return false;
                pre = pre->next;
                cur = cur->next;
            }
            return true;
            
        }
    };
    ```

- 快慢指针的写法
    + 更简洁一些，不需要通过计算链表的长度。
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
        bool isPalindrome(ListNode* head) {
            if(!head || !head->next)
                return true;
            ListNode* fst = head, *slow = head;
            while(fst->next && fst->next->next)
            {
                fst = fst->next->next;
                slow = slow->next;
            }
            ListNode* t = slow->next;
            while(t->next)
            {
                ListNode* last = t->next;
                t->next = last->next;
                last->next = slow->next;
                slow->next = last;
            }
            while(slow->next)
            {
                slow  = slow->next;
                if(head->val != slow->val)
                    return false;
                head = head->next;
            }
            return true;
            
        }
    };
    ```

- 解法二：递归
    + 二个参数，其中一个是指针的引用。
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
        bool isPalindrome(ListNode* head) {
            ListNode* cur = head;
            return help(head, cur);
        }
        bool help(ListNode* head, ListNode* &cur)
        {
            if(!head)
                return true;
            bool res = help(head->next, cur) && (head->val == cur->val);
            cur = cur->next;
            return res;
        }
    };
    ```