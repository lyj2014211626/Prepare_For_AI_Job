- [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [参考博客](https://github.com/grandyang/leetcode/issues/142)
- [官方解法](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode/)
- 解法一：双指针
    + 因为快指针每次走2，慢指针每次走1，快指针走的距离是慢指针的两倍。而快指针又比慢指针多走了一圈。所以 head 到环的起点+环的起点到他们相遇的点的距离 与 环一圈的距离相等。现在重新开始，head 运行到环起点 和 相遇点到环起点 的距离也是相等的，相当于他们同时减掉了 环的起点到他们相遇的点的距离。这里F=b代码如下：
    ![Alt text](https://pic.leetcode-cn.com/99987d4e679fdfbcfd206a4429d9b076b46ad09bd2670f886703fb35ef130635-image.png)
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
        ListNode *detectCycle(ListNode *head) {
            ListNode *low = head, *fast = head;

            while(fast != NULL && fast->next != NULL){
                low = low -> next;
                fast = fast -> next ->next;
                if(low == fast)
                    break;
            }
            if(fast == NULL || fast->next == NULL)//这里临界条件的判断重要
                return NULL;
            low = head;
            while(low != fast){
                low = low->next;
                fast = fast->next;
            }
            return fast;
        }
    };
    ```

- 解法二：集合法

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
        ListNode *detectCycle(ListNode *head) {
            unordered_set<ListNode*> s;
            ListNode * cur = head;
            while(cur){
                auto ind = s.find(cur);
                if(ind != s.end()){
                    return cur;
                }
                else{
                    s.insert(cur);
                    cur = cur->next;
                }
            }
            return NULL;
        }
    };
    ```
