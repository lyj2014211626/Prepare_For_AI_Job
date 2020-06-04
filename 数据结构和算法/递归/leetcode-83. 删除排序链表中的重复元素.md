- [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [参考博客](https://github.com/grandyang/leetcode/issues/83)
- [官方解法](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/solution/shan-chu-pai-xu-lian-biao-zhong-de-zhong-fu-yuan-s/)
- 解法一：双指针法
    + 用二个指针表示二个数，然后进行判断即可，分情况讨论
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
        ListNode* deleteDuplicates(ListNode* head) {
            if(!head)
                return head;
            ListNode *cur = head;
            ListNode *pre = cur->next;
            while(pre)
            {
                if(pre->val == cur->val)
                {
                    cur->next = pre->next;
                    pre = cur->next;
                }
                else
                {
                    cur = cur->next;
                    pre = pre->next;
                }
            }
            return head;
        }
    };
    ```

- 解法二：一个指针就可解决
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
        ListNode* deleteDuplicates(ListNode* head) {
            ListNode *cur = head;
            while(cur && cur->next)//条件判断
            {
                if(cur->val == cur->next->val)
                {
                    cur->next = cur->next->next;
                }
                else
                {
                    cur = cur ->next;
                }
            }
            return head;
        }
    };
```

- 解法三： 递归
    + 我们也可以使用递归的方法来做，首先判断是否至少有两个结点，若不是的话，直接返回 head。否则对 head->next **调用递归函数，并赋值给 head->next。这里可能比较晕**，先看后面一句，返回的时候，head 结点先跟其身后的结点进行比较，如果值相同，那么返回后面的一个结点，当前的 head 结点就被跳过了，而如果不同的话，还是返回 head 结点。可以发现了，进行实质上的删除操作是在最后一句进行了，再来看第二句，对 head 后面的结点调用递归函数，那么就应该 suppose 返回来的链表就已经没有重复项了，此时接到 head 结点后面，在第三句的时候再来检查一下 head 是否又 duplicate 了，实际上递归一直走到了末尾结点，再不断的回溯回来，进行删除重复结点，参见代码如下：
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
        ListNode* deleteDuplicates(ListNode* head) {
            if(!head || !head->next)
                return head;
            head->next = deleteDuplicates(head->next);
            return (head->val == head->next->val) ? head->next : head;
                
        }
    };
    ```