- [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
- [参考博客](https://github.com/grandyang/leetcode/issues/141)https://github.com/grandyang/leetcode/issues/141)
- 解法一：双指针
    + 这道题是快慢指针的经典应用。只需要设两个指针，一个每次走一步的慢指针和一个每次走两步的快指针，如果链表里有环的话，两个指针最终肯定会相遇。就是快指针总比慢指针快走一步。
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
        bool hasCycle(ListNode *head) {
            ListNode *low = head, *fast = head;
            while(fast != NULL && fast->next != NULL){//也进行了开始边界条件的判断
                low = low ->next;
                fast = fast ->next ->next;
                if(low == fast)
                    return true;
            }
            return false;
        }
    };
    ```

- 解法二：哈希集合
    + 用集合来存储任意元素的指针，继续向前遍历，如果找到相同的指针地址，则表示有换，否则无环。代码如下：
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
        bool hasCycle(ListNode *head) {
            unordered_set<ListNode*> s;
            ListNode *cur = head;
            while(cur){
                if(s.find(cur) != s.end())
                    return true;
                else{
                    s.insert(cur);
                    cur = cur->next;
                }
            }
            return false;
        }
    };
    ```