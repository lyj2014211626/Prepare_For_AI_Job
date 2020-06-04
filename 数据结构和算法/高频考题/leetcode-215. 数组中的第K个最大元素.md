- [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
- [参考博客](https://github.com/grandyang/leetcode/issues/215)
- [官方解法](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/solution/shu-zu-zhong-de-di-kge-zui-da-yuan-su-by-leetcode/)
- 解法一：快排思路，也是面试常考得问题
    + 这道题最好的解法应该是下面这种做法，用到了快速排序 Quick Sort 的思想，这里排序的方向是从大往小排。对快排不熟悉的童鞋们随意上网搜些帖子看下吧，多如牛毛啊，总有一款适合你。核心思想是每次都要先找一个中枢点 Pos，然后遍历其他所有的数字，像这道题从大往小排的话，就把大于中枢点的数字放到左半边，把小于中枢点的放在右半边，这样中枢点是整个数组中第几大的数字就确定了，虽然左右两部分各自不一定是完全有序的，但是并不影响本题要求的结果，因为左半部分的所有值都大于右半部分的任意值，所以我们求出中枢点的位置，如果正好是 k-1，那么直接返回该位置上的数字；如果大于 k-1，说明要求的数字在左半部分，更新右边界，再求新的中枢点位置；反之则更新右半部分，求中枢点的位置；不得不说，这个思路真的是巧妙啊～
    + C++版本
    ```
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k) {
            int left = 0;
            int right = nums.size() - 1;
            while(true)
            {
                if(left <= right)//这里的等号不能少 不然一个元素的通不过
                {
                    int pos = partition(nums, left, right);
                    if(pos == k - 1)
                        return nums[pos];
                    else if(pos < k - 1)
                        left = pos + 1;
                    else
                        right = pos - 1;
                }
            }
        }
        int partition(vector<int>& nums, int left, int right)
        {
            int l = left;
            int r = right;
            int key = nums[left];
            while(l < r)
            {
                while(l < r && nums[r] <= key)//找到第一个大于key的元素下表 这里和下面的先后顺序不能改 r先遍历完小于key的元素
                    --r;
                while(l < r && nums[l] >= key)//找到第一个小于Key的元素下标
                    ++l;
                swap(nums[l], nums[r]);
            }
            swap(nums[left], nums[l]);//不满足循环条件时  这里就将left和最后的 l == r元素调换
            return l;
        }
    };
    ```

- 解法二：堆
    + 下面这种解法是利用了 priority_queue （就是用堆数据结构来来实现的）的自动排序的特性，跟上面的解法思路上没有什么区别，当然我们也可以换成 multiset 来做，一个道理，参见代码如下：
    ```
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k) {
            priority_queue<int> q(nums.begin(), nums.end());
            for(int i = 0; i < k - 1; ++i)
                q.pop();
            return q.top();
        }
    };
    ```

- 解法三：排序
    + 将数组排序，然后取出第k个元素即可
    ```
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k) {
            sort(nums.begin(), nums.end());
            return nums[nums.size() - k];
        }
    };
    ```
