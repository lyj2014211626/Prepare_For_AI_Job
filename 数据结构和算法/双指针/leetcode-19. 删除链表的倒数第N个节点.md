- [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
- [参考博客](https://github.com/grandyang/leetcode/issues/19)
- [官方解法](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/solution/shan-chu-lian-biao-de-dao-shu-di-nge-jie-dian-by-l/)
- 解法一：
    + 我的思路是先新建一个哨兵头节点，然后下一个指针比前一个指针慢n + 1个出发即可。
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
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            ListNode *left = new ListNode(-1);
            left->next = head;
            ListNode *new_head = left;
            ListNode *right = head;
            int num = 0;

            while(right->next)
            {   
                ++num;
                right = right->next;
                if(num >= n)
                    left = left->next;
            }

            left->next = left->next->next;//这里很关键 不是直接指向right 因为right是遍历到链表尾的标志
            return new_head->next;
        }
    };
    ```

    + 这道题让我们移除链表倒数第N个节点，限定n一定是有效的，即n不会大于链表中的元素总数。还有题目要求一次遍历解决问题，那么就得想些比较巧妙的方法了。比如首先要考虑的时，如何找到倒数第N个节点，由于只允许一次遍历，所以不能用一次完整的遍历来统计链表中元素的个数，而是遍历到对应位置就应该移除了。那么就需要用两个指针来帮助解题，pre 和 cur 指针。**首先 cur 指针先向前走N步，如果此时 cur 指向空，说明N为链表的长度，则需要移除的为首元素，那么此时返回 head->next 即可，如果 cur 存在，再继续往下走**，此时 pre 指针也跟着走，直到 cur 为最后一个元素时停止，此时 pre 指向要移除元素的前一个元素，再修改指针跳过需要移除的元素即可，参见代码如下：
    + C++代码 **还是这个的思路好一点**
    ```
    class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (!head->next) return NULL;
        ListNode *pre = head, *cur = head;
        for (int i = 0; i < n; ++i) cur = cur->next;
        if (!cur) return head->next;
        while (cur->next) {
            cur = cur->next;
            pre = pre->next;
        }
        pre->next = pre->next->next;
        return head;
    }
};
    ```