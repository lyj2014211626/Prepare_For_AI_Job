- [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4249905.html)
- [官方解法](https://leetcode-cn.com/problems/sort-list/solution/sort-list-gui-bing-pai-xu-lian-biao-by-jyd/)
- 解法一：归并排序-自底向上的递归方法
    + 常见排序方法有很多，插入排序，选择排序，堆排序，快速排序，冒泡排序，归并排序，桶排序等等。。它们的时间复杂度不尽相同，而这里题目限定了时间必须为O(nlgn)，符合要求只有快速排序，归并排序，堆排序，而根据单链表的特点，最适于用归并排序。为啥呢？这是由于链表自身的特点决定的，由于不能通过坐标来直接访问元素，所以快排什么的可能不太容易实现（但是被评论区的大神们打脸，还是可以实现的），堆排序的话，如果让新建结点的话，还是可以考虑的，若只能交换结点，最好还是不要用。而归并排序（又称混合排序）因其可以利用递归来交换数字，天然适合链表这种结构。归并排序的**核心是一个 merge()** 函数，其主要是合并两个有序链表，这个在 LeetCode 中也有单独的题目 Merge Two Sorted Lists。由于两个链表是要有序的才能比较容易 merge，那么对于一个无序的链表，如何才能拆分成有序的两个链表呢？我们从简单来想，什么时候两个链表一定都是有序的？就是当两个链表各只有一个结点的时候，一定是有序的。而归并排序的核心其实是分治法 Divide and Conquer，就是将链表从中间断开，分成两部分，左右两边再分别调用排序的递归函数 sortList()，得到各自有序的链表后，再进行 merge()，这样整体就是有序的了。因为子链表的递归函数中还是会再次拆成两半，当拆到链表只有一个结点时，无法继续拆分了，而这正好满足了前面所说的“一个结点的时候一定是有序的”，**这样就可以进行 merge 了。然后再回溯回去**，每次得到的都是有序的链表，然后进行 merge，直到还原整个长度。这里**将链表从中间断开的方法，采用的就是快慢指针**，大家可能对快慢指针找链表中的环比较熟悉，其实找链表中的中点同样好使，因为快指针每次走两步，慢指针每次走一步，当快指针到达链表末尾时，慢指针正好走到中间位置，参见代码如下：
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
        ListNode* sortList(ListNode* head) {
            if(!head || !head->next)//空 或者只有一个链表的值 返回
                return head;
            ListNode *pre = head, *slow = head, *fast = head;//三个指针
            while(fast && fast->next)
            {
                pre = slow;
                slow = slow->next;
                fast = fast->next->next;    //到达尾指针 为了确定slow指针的位置的
            }
            pre->next = NULL;   //关键 上一个链表的尾指针置空
            return merge(sortList(head), sortList(slow));//递归调用
        }
        ListNode* merge(ListNode* l1, ListNode* l2)
        {
            ListNode *dummy = new ListNode(-1);
            ListNode *cur = dummy;
            while(l1 && l2)
            {
                if(l1->val < l2->val)
                {
                    cur->next = l1;
                    l1 = l1->next;
                }
                else
                {
                    cur->next = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            }
            if(l1)
                cur->next = l1;
            if(l2)
                cur->next = l2;
            return dummy->next;
        }
    };
    ```

- 解法一: Merge递归版
    + 下面这种方法也是归并排序，而且在merge函数中也使用了递归，这样使代码更加简洁啦～
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
        ListNode* sortList(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode *pre = head, *slow = head, *fast = head;
            while(fast && fast->next)
            {
                pre = slow;
                slow = slow->next;
                fast = fast->next->next;
            }
            pre->next = NULL;
            return merge(sortList(head), sortList(slow));
        }
        ListNode* merge(ListNode* l1, ListNode* l2)
        {
            if(!l1)
                return l2;
            if(!l2)
                return l1;
            if(l1->val < l2->val)
            {
                l1->next = merge(l1->next, l2);
                return l1;
            }
            else
            {
                l2->next = merge(l1, l2->next);
                return l2;
            }
        }
    };
    ```

- 解法二：归并排序-自底向上的迭代方法
    + [**参考**](https://leetcode-cn.com/problems/sort-list/comments/)
    + bottom-to-up 的归并思路是这样的：先两个两个的 merge，完成一趟后，再 4 个4个的 merge，直到结束。举个简单的例子：[4,3,1,7,8,9,2,11,5,6].
    ```
    step=1: (3->4)->(1->7)->(8->9)->(2->11)->(5->6)
    step=2: (1->3->4->7)->(2->8->9->11)->(5->6)
    step=4: (1->2->3->4->7->8->9->11)->5->6
    step=8: (1->2->3->4->5->6->7->8->9->11)
    ```
    + **链表里操作最难掌握的应该就是各种断链啊，然后再挂接啊**。在这里，我们主要用到链表操作的两个技术：
    + merge(l1, l2)，双路归并，我相信这个操作大家已经非常熟练的，就不做介绍了。cut(l, n)，可能有些同学没有听说过，它其实就是一种 split 操作，即断链操作。不过我感觉使用 **cut 更准确一些，它表示，将链表 l 切掉前 n 个节点，并返回后半部分的链表头**。
    + 额外再补充一个 dummyHead 大法，已经讲过无数次了，仔细体会吧。
    + 希望同学们能把双路归并和 cut 断链的代码烂记于心，以后看到类似的题目能够刷到手软。
    + 掌握了这三大神器后，我们的 bottom-to-up 算法伪代码就十分清晰了：
    ```
    current = dummy.next;
    tail = dummy;
    for (step = 1; step < length; step *= 2) {
        while (current) {
            // left->@->@->@->@->@->@->null
            left = current;

            // left->@->@->null   right->@->@->@->@->null
            right = cut(current, step); // 将 current 切掉前 step 个头切下来。

            // left->@->@->null   right->@->@->null   current->@->@->null
            current = cut(right, step); // 将 right 切掉前 step 个头切下来。
            
            // dummy.next -> @->@->@->@->null，最后一个节点是 tail，始终记录
            //                        ^
            //                        tail
            tail.next = merge(left, right);
            while (tail->next) tail = tail->next; // 保持 tail 为尾部
        }
    }
    ```
+ **归并的迭代版本**
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
        ListNode* sortList(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode* dummy = new ListNode(-1);
            dummy->next = head;
            ListNode *p = head;
            int len = 0;
            while(p)    //计算链表的长度
            {
                ++len;
                p = p->next;
            }
            for(int i = 1; i < len; i = 2 * i)
            {
                ListNode* cur = dummy->next;
                ListNode* tail = dummy;
                while(cur)
                {
                    ListNode* left = cur;
                    ListNode* right = cut(cur, i);
                    cur = cut(right, i);
                    tail->next = merge(left, right);
                    while(tail->next)   //每一次合并后链表的最后一个元素
                        tail = tail->next;
                }
            }
            return dummy->next;
            
        }
        ListNode* cut(ListNode*p, int n)//返回从p节点切割后的下个链表的头节点
        {
            while(--n && p)
            {
                p = p->next;
            }
            if(!p)
                return NULL;
            ListNode* next = p->next;
            p->next = NULL;
            return next;
        }
        ListNode* merge(ListNode* l1, ListNode* l2)
        {
            ListNode *dummy = new ListNode(-1);
            ListNode *cur = dummy;
            while(l1 && l2)
            {
                if(l1->val < l2->val)
                {
                    cur->next = l1;
                    l1 = l1->next;
                }
                else
                {
                    cur->next = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            }
            if(l1)
                cur->next = l1;
            if(l2)
                cur->next = l2;
            return dummy->next;
        }
    };
```

- 解法三：快排的思想
    + [**参考的链接**](https://blog.csdn.net/wumuzi520/article/details/8078322)
    + 你可能回诧异，怎么会是快排？快排不是需要一个指针指向头，一个指针指向尾，然后两个指针相向运动并按一定规律交换值，最后找到一个支点使得支点左边小于支点，支点右边大于支点吗（这句话很长，累死俺了）？是滴，木有错，不过问题出来了。如果是这样的话，对于单链表我们没有前驱指针，怎么能使得后面的那个指针往前移动呢？所以这种快排思路行不通滴，如果我们能使两个指针都往next方向移动并且能找到支点那就好了。怎么做呢？
    + 接下来我们**使用快排的另一种思路来解答**。我们只需要两个指针p和q，这两个指针均往next方向移动，**移动的过程中保持p之前的key都小于选定的key，p和q之间的key都大于选定的key**，那么当q走到末尾的时候便完成了一次支点的寻找。如下图所示：
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
        ListNode* sortList(ListNode* head) {
            if(!head || !head->next)
                return head;
            quickSort(head, NULL);
            return head;
        }
        void quickSort(ListNode* begin, ListNode* end)
        {
            if(begin != end)
            {
                ListNode* pos = partision(begin, end);
                quickSort(begin, pos);
                quickSort(pos->next, end);
            }
        }
        ListNode* partision(ListNode*pbegin, ListNode* pend)//这个函数很重要
        {
            int key = pbegin->val;
            ListNode* p = pbegin;
            ListNode* q = p ->next;
            while(q != pend)    //保证移动的过程中保持p之前的key都小于选定的key，p和q之间的key都大于选定的key
            {
                if(q->val < key)
                {
                    p = p->next;
                    swap(p->val, q->val);
                }
                q = q->next;
            }
            swap(p->val, pbegin->val);
            return p;
        }

    };
    ```