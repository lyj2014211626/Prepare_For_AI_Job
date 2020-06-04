- [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4261431.html)
- [官方解法](https://leetcode-cn.com/problems/copy-list-with-random-pointer/solution/fu-zhi-dai-sui-ji-zhi-zhen-de-lian-biao-by-leetcod/)
- 解法一：迭代+ 哈希表
    + next指针的顺序赋值很简单，主要是难在random的下一个元素定位的问题上。
    + 如果原始列表节点N的random指向S,则在复制的链表节点N'的random指向S'。则需要添加一个哈希表，使得S指向S'。
    + 时间复杂度和空间复杂度都是O(N)。
    ```C++
    /*
    // Definition for a Node.
    class Node {
    public:
        int val;
        Node* next;
        Node* random;
        
        Node(int _val) {
            val = _val;
            next = NULL;
            random = NULL;
        }
    };
    */

    class Solution {
    public:
        Node* copyRandomList(Node* head) {
            if(!head)
                return NULL;
            Node* res = new Node(head->val);
            Node* node = res;
            Node* cur = head->next;
            unordered_map<Node*, Node*> m;
            m[head] = res;
            while(cur)
            {
                Node* t = new Node(cur->val);
                node->next = t;
                m[cur] = t;
                cur = cur->next;
                node = node->next;
            }
            node = res;
            cur = head;
            while(cur)
            {
                node->random = m[cur->random];
                cur = cur->next;
                node = node->next;
            }
            return res;
        }
    };
    ```

- 解法二：递归 + 哈希
    + 我们可以使用递归的解法，写起来相当的简洁，还是需要一个 HashMap 来建立原链表结点和拷贝链表结点之间的映射。在递归函数中，首先判空，若为空，则返回空指针。然后就是去 HashMap 中查找是否已经在拷贝链表中存在了该结点，是的话直接返回。否则新建一个拷贝结点 res，然后建立原结点和该拷贝结点之间的映射，然后就是要给拷贝结点的 next 和 random 指针赋值了，直接分别调用递归函数即可，参见代码如下：
    ```C++
    /*
    // Definition for a Node.
    class Node {
    public:
        int val;
        Node* next;
        Node* random;
        
        Node(int _val) {
            val = _val;
            next = NULL;
            random = NULL;
        }
    };
    */

    class Solution {
    public:
        Node* copyRandomList(Node* head) {
            if(!head)
                return NULL;
            unordered_map<Node*, Node*> m;
            return cursion(head, m);
        }
        Node* cursion(Node* node, unordered_map<Node*, Node*>& m)
        {
            if(!node)
                return NULL;
            if(m.count(node))
                return m[node];
            Node* res = new Node(node->val);
            m[node] = res;
            res->next = cursion(node->next, m);
            res->random = cursion(node->random, m);
            return res;
        }
    };
    ```

- 解法三：常数空间和线性时间
    + 当然，如果使用 HashMap 占用额外的空间，如果这道题限制了空间的话，就要考虑别的方法。下面这个方法很巧妙，可以分为以下三个步骤：
    
    + 1. 在原链表的每个节点后面拷贝出一个新的节点。

    + 2. 依次给新的节点的随机指针赋值，而且这个赋值非常容易 cur->next->random = cur->random->next。

    + 3. 断开链表可得到深度拷贝后的新链表。

    + 举个例子来说吧，比如原链表是 1(2) -> 2(3) -> 3(1)，括号中是其 random 指针指向的结点，那么这个解法是首先比遍历一遍原链表，在每个结点后拷贝一个同样的结点，但是拷贝结点的 random 指针仍为空，则原链表变为 1(2) -> 1(null) -> 2(3) -> 2(null) -> 3(1) -> 3(null)。然后第二次遍历，是将拷贝结点的 random 指针赋上正确的值，则原链表变为 1(2) -> 1(2) -> 2(3) -> 2(3) -> 3(1) -> 3(1)，注意赋值语句为：
```
cur->next->random = cur->random->next;
```

    - 这里的 cur 是原链表中结点，cur->next 则为拷贝链表的结点，cur->next->random 则为拷贝链表的 random 指针。cur->random 为原链表结点的 random 指针指向的结点，因为其指向的还是原链表的结点，所以我们要再加个 next，才能指向拷贝链表的结点。最后再遍历一次，就是要把原链表和拷贝链表断开即可，参见代码如下：
    ```C++
    /*
    // Definition for a Node.
    class Node {
    public:
        int val;
        Node* next;
        Node* random;
        
        Node(int _val) {
            val = _val;
            next = NULL;
            random = NULL;
        }
    };
    */

    class Solution {
    public:
        Node* copyRandomList(Node* head) {
            if(!head)
                return NULL;
            Node* cur = head;
            while(cur)      //复制链接节点
            {
                Node* t = new Node(cur->val);
                t ->next = cur->next;
                cur->next = t;
                cur = t->next;
            }
            cur = head;
            while(cur)  //复制random
            {
                if(cur->random)
                    cur->next->random = cur->random->next;
                cur = cur->next->next;
            }
            cur = head; //断开链接
            Node* res = head->next;
            while(cur)
            {
                Node* t = cur->next;
                cur->next = t->next;
                if(t->next)
                    t->next = t->next->next;
                cur = cur->next;
            }
            return res;
        }
    };
    ```