- [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
- [参考解法](https://leetcode-cn.com/problems/sliding-window-maximum/)
- [官方解法](https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetcode-3/)
- 解法一：双向单调最大队列
    + 一个滑动窗口可以看作是一个队列，当滑动窗口滑动时，符合队列的先进先出的性质。
    + 题目中的 Follow up 要求我们代码的时间复杂度为 O(n)。提示我们要用双向队列 deque 来解题，并提示我们窗口中只留下有用的值，没用的全移除掉。果然 Hard 的题目我就是不会做，网上看到了别人的解法才明白，解法又巧妙有简洁，膜拜啊。大概思路是用双向队列保存数字的下标，遍历整个数组，如果此时队列的首元素是 i-k 的话，表示此时窗口向右移了一步，则移除队首元素。然后比较队尾元素和将要进来的值，如果小的话就都移除，然后此时我们把队首元素加入结果中即可，参见代码如下：
    ```C++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
            deque<int> dq;
            for(int i = 0; i < nums.size(); ++i)
            {
                if(!dq.empty() && i - k == dq.front())
                    dq.pop_front();
                while(!dq.empty() && nums[i] > nums[dq.back()])
                    dq.pop_back();
                dq.push_back(i);
                if(i >= k - 1)
                    res.push_back(nums[dq.front()]);
            }
            return res;
        }
    };
    ```

- 解法二：用集合
    ```C++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
            multiset<int> s;
            for(int i = 0; i < nums.size(); ++i)
            {
                if(i >= k)
                    s.erase(s.find(nums[i - k]));
                s.insert(nums[i]);
                if(i >= k - 1)
                    res.push_back(*s.rbegin());
                
            }
            return res;
        }
    };
    ```
