- [946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)
- [参考解法](https://leetcode-cn.com/problems/validate-stack-sequences/solution/cmo-ni-chu-zhan-ru-zhan-by-yizhe-shi/)
- [参考解法](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/solution/tan-xin-by-z1m/)
- 解法一：
    + 我们只要模拟这个过程就好了，先按 pushed 数组入栈，当入栈元素等于 popped 数组对应的数字之后，把该元素出栈，最后如果正确的话栈肯定是空的。
    ```C++
    class Solution {
    public:
        bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
            if(pushed.size() != popped.size())
                return false;
            stack<int> s;
            int idx1 = 0, idx2 = 0;
            while(idx1 < pushed.size() && idx2 < popped.size())
            {
                if(idx1 < pushed.size())
                {
                    s.push(pushed[idx1]);
                    ++idx1;
                }
                while(idx2 < popped.size() && !s.empty() && s.top() == popped[idx2])//三个条件缺一不可
                {
                    s.pop();
                    ++idx2;
                }
            }
            return s.empty();
        }
    };
    ```

    + 简化版本
    + 直接遍历pushed数组的元素，全部进栈，然后弹出所有相同序列的数值
    ```C++
    class Solution {
    public:
        bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
            if(pushed.size() != popped.size())
                return false;
            stack<int> s;
            int idx2 = 0;
            for(auto item : pushed)
            {
                s.push(item);
                while(idx2 < popped.size() && !s.empty() && s.top() == popped[idx2])
                {
                    s.pop();
                    ++idx2;
                }
            }
            return s.empty();
        }
    };
    ```