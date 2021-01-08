- [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)
- 解法一：滑动窗口
    ```C++
    class MaxQueue {
    public:
        MaxQueue() {

        }
        
        int max_value() {
            if(dq.empty())
                return -1;
            return val.front();
        }
        
        void push_back(int value) {
            dq.push_back(value);
            while(!val.empty() && value > val.back())
                val.pop_back();
            val.push_back(value);
        }
        
        int pop_front() {
            if(dq.empty())
            {
                return -1;
            }
            int t = dq.front();
            dq.pop_front();
            if(t == val.front())
                val.pop_front();
            return t;
        }
    private:
        deque<int> dq;
        deque<int> val;
    };

    /**
     * Your MaxQueue object will be instantiated and called as such:
     * MaxQueue* obj = new MaxQueue();
     * int param_1 = obj->max_value();
     * obj->push_back(value);
     * int param_3 = obj->pop_front();
     */
    ```
