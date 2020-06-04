- [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4606710.html)
- [官方解法](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode/)
- 解法一：就进行二二链表之间的合并即可
    + 对链表进行两两之间的合并。下面是我自己手写的代码。
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
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            if(lists.empty())
                return NULL;
            ListNode* head = lists[0];
            for(int i = 1; i < lists.size(); ++i)
            {
                head = combine(head, lists[i]);
            }
            return head;
        }
        ListNode* combine(ListNode *head1, ListNode* head2)
        {
            ListNode * head = new ListNode(-1);
            ListNode* cur = head;
            while(head1 && head2)
            {
                if(head1->val < head2->val)
                {
                    cur->next = head1;
                    head1 = head1->next;
                }
                else
                {
                    cur->next = head2;
                    head2 = head2->next;
                }
                cur = cur->next;
            }
            cur->next = head1 ? head1 : head2;
            return head->next;
        }
    };
    ```
- 解法二：分治
    + 这道题让我们合并k个有序链表，最终合并出来的结果也必须是有序的，之前做过一道 [Merge Two Sorted Lists](https://www.cnblogs.com/grandyang/p/4086297.html)，是混合插入两个有序链表。这道题增加了难度，变成合并k个有序链表了，**但是不管合并几个，基本还是要两两合并。**那么首先考虑的方法是能不能利用之前那道题的解法来解答此题。答案是肯定的，但是需要修改，怎么修改呢，最先想到的就是两两合并，就是前两个先合并，合并好了再跟第三个，然后第四个直到第k个。这样的思路是对的，但是效率不高，没法通过 OJ，所以只能换一种思路，这里就需要用到**分治法 Divide and Conquer Approach**。简单来说就是不停的对半划分，比如**k个链表先划分为合并两个 k/2 个链表的任务，再不停的往下划分，直到划分成只有一个或两个链表的任务，开始合并**。举个例子来说比如合并6个链表，那么按照分治法，首先分别合并0和3，1和4，2和5。这样下一次只需合并3个链表，再合并1和3，最后和2合并就可以了。代码中的k是通过 (n+1)/2 计算的，这里为啥要加1呢，这是为了当n为奇数的时候，k能始终从后半段开始，比如当 n=5 时，那么此时 k=3，则0和3合并，1和4合并，最中间的2空出来。当n是偶数的时候，加1也不会有影响，比如当 n=4 时，此时 k=2，那么0和2合并，1和3合并，完美解决问题，参见代码如下：
    
    ```C++
    class Solution {
    public:
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            if (lists.empty()) return NULL;
            int n = lists.size();
            while (n > 1) {
                int k = (n + 1) / 2;
                for (int i = 0; i < n / 2; ++i) {
                    lists[i] = mergeTwoLists(lists[i], lists[i + k]);
                }
                n = k;
            }
            return lists[0];
        }
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            ListNode *dummy = new ListNode(-1), *cur = dummy;
            while (l1 && l2) {
                if (l1->val < l2->val) {
                    cur->next = l1;
                    l1 = l1->next;
                } else {
                    cur->next = l2;
                    l2 = l2->next;
                }
                cur = cur->next;
            }
            if (l1) cur->next = l1;
            if (l2) cur->next = l2;
            return dummy->next;
        }
    };
    ```

- 解法三：堆结构
    + 我们再来看另一种解法，这种解法利用了最小堆这种数据结构，首先把k个链表的首元素都加入最小堆中，它们会自动排好序。然后每次取出最小的那个元素加入最终结果的链表中，然后把取出元素的下一个元素再加入堆中，下次仍从堆中取出最小的元素做相同的操作，以此类推，直到堆中没有元素了，此时k个链表也合并为了一个链表，返回首节点即可，参见代码如下：
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
        struct cmp
        {
            bool operator()( ListNode* a, ListNode*& b)
            {
                return a->val > b->val;
            }
        };
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            if(lists.empty())
                return NULL;
            priority_queue<ListNode*, vector<ListNode*>, cmp> q;
            ListNode *head = new ListNode(-1);
            ListNode *cur = head;
            for(auto node : lists)//首先堆每个链表得首元素进队列
            {
                if(node)
                    q.push(node);
            }
            while(!q.empty())
            {
                auto t = q.top();
                q.pop();
                cur->next = t;
                cur = cur->next;
                if(t->next) //当前元素进队列 保证每个链表就你一个元素
                    q.push(t->next);
            }
            return head->next;
        }

    };
    ```

- 解法四：递归版的分治
    + 下面这种解法利用到了混合排序的思想，也属于分治法的一种，做法是将原链表分成两段，然后对每段调用递归函数，suppose 返回的 left 和 right 已经合并好了，然后再对 left 和 right 进行合并，合并的方法就使用之前那道 Merge Two Sorted Lists 中的任意一个解法即可，这里使用了递归的写法，而本题解法一中用的是迭代的写法，参见代码如下：
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
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            if(lists.empty())
                return NULL;
            int start = 0;
            int end = lists.size() - 1;
            return cursion(start, end, lists);
        }
        ListNode* cursion(int start, int end, vector<ListNode*>& lists)
        {
            if(start > end)
                return NULL;
            if(start == end)
                return lists[start];
            int mid = start + (end - start) / 2;
            ListNode* left = cursion(start, mid, lists);
            ListNode* right = cursion(mid + 1, end, lists);
            return merge(left, right);
        }
        ListNode* merge(ListNode* L1, ListNode* L2)
        {
            if(!L1)
                return L2;
            if(!L2)
                return L1;
            if(L1->val < L2->val)
            {
                L1->next = merge(L1->next, L2);
                return L1;
            }
            else
            {
                L2->next = merge(L1, L2->next);
                return L2;
            }
            
        }

    };
    ```