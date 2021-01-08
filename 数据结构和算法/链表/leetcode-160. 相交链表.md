- [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4128461.html)
- [官方解法](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/solution/xiang-jiao-lian-biao-by-leetcode/)
- 解法一
    + 通过计算二个链表的长度，然后长的链表向前移动几个即可。
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
        ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            if (headA == NULL || headB == NULL) return NULL;
            int lenA = getLen(headA);
            int lenB = getLen(headB);
            if(lenA < lenB)
            {
                for(int i = 0; i < (lenB - lenA); ++i)
                    headB = headB->next;
            }
            else
            {
                for(int  i = 0; i < (lenA - lenB); ++i)
                {
                    headA = headA->next;
                }
            }
            while(headA != NULL && headB != NULL && headA != headB)
            {
                headA = headA->next;
                headB = headB->next;
            }
            return headA;
        }
        int getLen(ListNode* head)
        {
            int len = 0;
            while(head)
            {
                ++len;
                head = head->next;
            }
            return len;
        }
    };
    ```

- 解法二：双指针
    + 这道题还有一种特别巧妙的方法，虽然题目中强调了链表中不存在环，但是我们可以用环的思想来做，我们让两条链表分别从各自的开头开始往后遍历，当其中一条遍历到末尾时，我们跳到另一个条链表的开头继续遍历。两个指针最终会相等，而且只有两种情况，一种情况是在交点处相遇，另一种情况是在各自的末尾的空节点处相等。为什么一定会相等呢，因为两个指针走过的路程相同，是两个链表的长度之和，所以一定会相等。这个思路真的很巧妙，而且更重要的是代码写起来特别的简洁，参见代码如下：
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
        ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            if (headA == NULL || headB == NULL) return NULL;
            ListNode* a = headA;
            ListNode* b = headB;
            while(a != b)
            {
                a = a ? a->next : headB;
                b = b ? b->next : headA;
            }
            return a;
        }
    };
    ```