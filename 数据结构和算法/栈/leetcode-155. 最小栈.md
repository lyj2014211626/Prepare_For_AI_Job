- [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4091064.html)
- [官方解法](https://leetcode-cn.com/problems/min-stack/solution/zui-xiao-zhan-by-leetcode-solution/)
- 解法一
    + 设计二个栈，一个保存当前数值，一个保存目前数据为止最下值。
    + 入栈的时候，判断当前值和最小栈顶元素的大小关系，选择最小栈是否进元素。
    + 出栈的时候，判断二个栈的当前元素是否相等，而选择最小栈是否弹出。
    ```C++
    class MinStack {
    public:
        /** initialize your data structure here. */
        MinStack() {
            
        }
        
        void push(int x) {
            s.push(x);
            if(min_s.empty() || x <= min_s.top())
                min_s.push(x);
        }
        
        void pop() {
            if(s.top() == min_s.top())
                min_s.pop();
            s.pop();
            
        }
        
        int top() {
            return s.top();
        }
        
        int getMin() {
            return min_s.top();
        }
    private:
        stack<int> s, min_s;
    };

    /**
     * Your MinStack object will be instantiated and called as such:
     * MinStack* obj = new MinStack();
     * obj->push(x);
     * obj->pop();
     * int param_3 = obj->top();
     * int param_4 = obj->getMin();
     */
    ```

- 解法二
    + 主要区别在于每次是否重复对最小栈中Push最小元素。
    ```C++
    class MinStack {
    public:
        /** initialize your data structure here. */
        MinStack() {
            min_s.push(INT_MAX);
        }
        
        void push(int x) {
            s.push(x);
            min_s.push(min(x, min_s.top()));
        }
        
        void pop() {
            min_s.pop();
            s.pop();
            
        }
        
        int top() {
            return s.top();
        }
        
        int getMin() {
            return min_s.top();
        }
    private:
        stack<int> s, min_s;
    };

    /**
     * Your MinStack object will be instantiated and called as such:
     * MinStack* obj = new MinStack();
     * obj->push(x);
     * obj->pop();
     * int param_3 = obj->top();
     * int param_4 = obj->getMin();
     */
    ```