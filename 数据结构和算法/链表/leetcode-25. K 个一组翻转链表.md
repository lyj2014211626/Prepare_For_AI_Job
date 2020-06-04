- [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
- [参考博客](https://github.com/grandyang/leetcode/issues/25)
- [官方解答](https://github.com/grandyang/leetcode/issues/25)
- 解法一
    + 这道题让我们以每k个为一组来翻转链表，实际上是把原链表分成若干小段，然后分别对其进行翻转，那么肯定总共需要两个函数，一个是用来分段的，一个是用来翻转的，以题目中给的例子来看，对于给定链表 1->2->3->4->5，一般在处理链表问题时，大多时候都会在开头再加一个 dummy node，因为翻转链表时头结点可能会变化，为了记录当前最新的头结点的位置而引入的 dummy node，加入 dummy node 后的链表变为 -1->1->2->3->4->5，如果k为3的话，目标是将 1,2,3 翻转一下，那么需要一些指针，**pre 和 next 分别指向要翻转的链表的前后的位置**，等价成单链表的反转问题，对应的分别是哨兵头节点指针和尾空指针，[而单链表的反转又三种方式，分别是三个指针直接反转和二个指针的后指针反转和递归](https://blog.csdn.net/feliciafay/article/details/6841115)。然后翻转后 pre 的位置更新到如下新的位置：
    ```
    -1->1->2->3->4->5
     |        |  |
    pre      cur next

    -1->3->2->1->4->5
        |     |  |
       cur   pre next
    ```
    + 以此类推，只要 cur 走过k个节点，那么 next 就是 cur->next，就可以调用翻转函数来进行局部翻转了，注意翻转之后新的 cur 和 pre 的位置都不同了，那么翻转之后，cur 应该更新为 pre->next，而如果不需要翻转的话，cur 更新为 cur->next，代码如下所示：
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
        ListNode* reverseKGroup(ListNode* head, int k) {
            if(!head || k == 1)
                return head;
            ListNode *dummy = new ListNode(-1);//定义哨兵节点
            dummy->next = head;
            ListNode *pre = dummy;
            ListNode *cur = head;
            for(int i = 1; cur; ++i)
            {
                if(i % k == 0)
                {
                    pre = reverse(pre, cur->next);//调用反转函数 注意是cur-next
                    cur = pre->next;
                }
                else
                {
                    cur = cur->next;
                }
            }
            return dummy->next;
        }
        ListNode* reverse(ListNode *pre, ListNode *next)
        {
            ListNode *last = pre->next;
            ListNode *cur = last->next;
            while(cur != next)//这里是将cur节点提到pre后面 完成反转 四个语句 公式给记住
            {
                last->next = cur->next;
                cur->next = pre->next;
                pre->next = cur;
                cur = last->next;
            }
            return last;
            
        }
    };
    ```

- 解法二
    + 我们也可以在一个函数中完成，首先遍历整个链表，统计出链表的长度，然后如果长度大于等于k，交换节点，当 k=2 时，每段只需要交换一次，当 k=3 时，每段需要交换2此，所以i从1开始循环，注意交换一段后更新 pre 指针，然后 num 自减k.
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
        ListNode* reverseKGroup(ListNode* head, int k) {
            if(!head || k == 1)
                return head;
            ListNode *dummy = new ListNode(-1);
            dummy->next = head;
            ListNode *pre = dummy;
            ListNode *cur = pre;
            int num = 0;
            while(cur = cur->next)
            {
                ++num;
                if(num % k == 0)
                {
                    cur = pre->next;
                    for(int i = 1; i < k; ++i)//也是采用尾节点插入到首节点的方式进行反转
                    {
                        ListNode *t = cur->next;
                        cur->next = t->next;
                        t->next = pre->next;
                        pre->next = t;
                    }
                    pre = cur;//更换
                    //num -= k;
                }
            }
            return dummy->next;
        }

    };
    ```

- 解法三：递归
    + 我们也可以使用递归来做，用 head 记录每段的开始位置，cur 记录结束位置的下一个节点，然后调用 reverse 函数来将这段翻转，然后得到一个 new_head，原来的 head 就变成了末尾，这时候后面接上递归调用下一段得到的新节点，返回 new_head 即可，参见代码如下：
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
        ListNode* reverseKGroup(ListNode* head, int k) {
            ListNode *cur = head;
            for(int i = 0; i < k; ++i)
            {
                if(cur == NULL)
                    return head;
                cur = cur -> next;
            }
            ListNode *new_head = reverse(head, cur);
            head->next = reverseKGroup(cur, k);
            return new_head;
        }
        
        ListNode *reverse(ListNode *head, ListNode *tail)
        {
            ListNode *pre = tail;
            while(head != tail)
            {
                ListNode *t = head->next;
                head->next = pre;
                pre = head;
                head = t;
            }
            return pre;
        }

    };
    ```