- [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4478820.html)
- [官方解法](https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode/)
- 关于链表求逆的几种解法
    + [全面分析再动手的习惯：链表的反转问题（递归和非递归方式）](https://www.cnblogs.com/kubixuesheng/p/4394509.html)
    + [看图理解单链表的反转](https://blog.csdn.net/feliciafay/article/details/6841115)
- 解法一：递归
    + 这里有一个很巧妙的地方就是头节点的返回问题，这里每一个父函数返回的结果都是上一个子函数返回的结果。
    + 我子节点下的所有节点都已经反转好了，现在就剩我和我的子节点 没有完成最后的反转了，所以反转一下我和我的子节点。
    + 代码里的每一行都是很关键
    
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
            ListNode* newHead = reverseList(head->next);
            head->next->next = head;
            head->next = NULL;
            return newHead;
        }
    };
    ```

- 迭代
- 二个指针解法：从后向前一个一个移动后面的节点达到求逆的过程。
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
            ListNode *pre = new ListNode(-1);
            pre->next = head;
            ListNode *cur = head;
            while(cur->next)
            {
                ListNode*t = cur->next;
                cur->next = t->next;
                t->next = pre->next;
                pre->next = t;
            }
            return pre->next;
        }
    };
    ```

- 三指针解法
    - 从前往后依次进行求逆。
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
            ListNode* newHead = NULL;
            while(head)
            {
                ListNode* t = head->next;
                head->next = newHead;
                newHead = head;
                head = t;
            }
            return newHead;
        }
    };
    ```