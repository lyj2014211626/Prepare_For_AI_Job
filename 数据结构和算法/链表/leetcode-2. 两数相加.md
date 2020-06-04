- [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
- [参考博客](https://github.com/grandyang/leetcode/issues/2)
- [官方答案](https://leetcode-cn.com/problems/add-two-numbers/solution/liang-shu-xiang-jia-by-leetcode/)
- 解法一
    + 这道并不是什么难题，算法很简单，链表的数据类型也不难，就是建立一个新链表，然后把输入的两个链表从头往后撸，每两个相加，添加一个新节点到新链表后面。为了避免两个输入链表同时为空，我们建立一个 dummy 结点，将两个结点相加生成的新结点按顺序加到 dummy 结点之后，由于 dummy 结点本身不能变，所以用一个指针 cur 来指向新链表的最后一个结点。好，可以开始让两个链表相加了，这道题好就好在最低位在链表的开头，所以可以在遍历链表的同时按从低到高的顺序直接相加。while 循环的条件两个链表中只要有一个不为空行，由于链表可能为空，所以在取当前结点值的时候，先判断一下，若为空则取0，否则取结点值。然后把两个结点值相加，同时还要加上进位 carry。然后更新 carry，直接 sum/10 即可，然后以 sum%10 为值建立一个新结点，连到 cur 后面，然后 cur 移动到下一个结点。之后再更新两个结点，若存在，则指向下一个位置。while 循环退出之后，最高位的进位问题要最后特殊处理一下，若 carry 为1，则再建一个值为1的结点，代码如下：
    + C++版本 我写的 明显有点冗余了 
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
        ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
            ListNode *pre = new ListNode(-1);
            pre->next = NULL;
            int flag = 0;
            ListNode * cur = pre;
            while(l1 && l2)
            {
                int value = l1->val + l2->val + flag;
                flag = value / 10;
                ListNode * t = new ListNode(value % 10);
                cur->next = t;//不用将生成的新节点的指向空 是因为初始化的时候就已经时空了
                cur = t;
                l1 = l1->next;
                l2 = l2->next;
            }
            while(l1)
            {
                int value = l1->val + flag;
                flag = value / 10;
                ListNode * t = new ListNode(value % 10);
                cur->next = t;
                cur = t;
                l1 = l1->next;
            }
            
            while(l2)
            {
                int value = l2->val + flag;
                flag = value / 10;
                ListNode * t = new ListNode(value % 10);
                cur->next = t;
                cur = t;
                l2 = l2->next;
            }
            if(flag)
            {
                ListNode * t = new ListNode(flag);
                cur->next = t;
                cur = t;
            }
            return pre->next;
        }
    };
    ```

    + C++版本的，**比较简洁版本，可以学习**
    ```
    class Solution {
    public:
        ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
            ListNode *dummy = new ListNode(-1), *cur = dummy;
            int carry = 0;
            while (l1 || l2) {
                int val1 = l1 ? l1->val : 0;//这里的处理很好
                int val2 = l2 ? l2->val : 0;
                int sum = val1 + val2 + carry;
                carry = sum / 10;
                cur->next = new ListNode(sum % 10);//直接指向生成的新节点
                cur = cur->next;
                if (l1) l1 = l1->next;
                if (l2) l2 = l2->next;
            }
            if (carry) cur->next = new ListNode(1);//当进位不为0时
            return dummy->next;
        }
    };
    ```