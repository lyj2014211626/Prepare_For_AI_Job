- [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)
- [参考博客](https://github.com/grandyang/leetcode/issues/347)
- [官方解答](https://leetcode-cn.com/problems/top-k-frequent-elements/solution/qian-k-ge-gao-pin-yuan-su-by-leetcode/)
- 解法一：哈希表+优先队列
    + 这道题给了我们一个数组，让我们统计前k个高频的数字，那么对于这类的统计数字的问题，首先应该考虑用 HashMap 来做，建立数字和其出现次数的映射，然后再按照出现次数进行排序。我们可以用堆排序来做，使用一个最大堆来按照映射次数从大到小排列，在 C++ 中使用 priority_queue 来实现，默认是最大堆，参见代码如下：
    + 这里优先队列的内置int类型初始化是0，[详细可看这里](https://blog.csdn.net/dreamiond/article/details/84101516)
    + C++版本
    ```
    class Solution {
    public:
        vector<int> topKFrequent(vector<int>& nums, int k) {
            unordered_map<int, int> tab;
            priority_queue<pair<int, int>> node;
            vector<int>res;
            for(int item : nums)
                tab[item]++;
            for(auto it : tab)
                node.push({it.second, it.first});

            for(int i = 0; i < k; ++i)
            {
                res.push_back(node.top().second);
                node.pop();
            }
            return res;
            
        }
    };
    ```

- 解法二：桶排序
    + 我们还可以使用桶排序，在建立好数字和其出现次数的映射后，我们按照其出现次数将数字放到对应的位置中去，这样我们从桶的后面向前面遍历，最先得到的就是出现次数最多的数字，我们找到k个后返回即可，参见代码如下：
    + C++版本
    ```
    class Solution {
    public:
        vector<int> topKFrequent(vector<int>& nums, int k) {
            unordered_map<int, int> m;
            vector<vector<int>> bucket(nums.size() + 1);
            vector<int> res;
            for (auto a : nums) ++m[a];
            for (auto it : m) {
                bucket[it.second].push_back(it.first);
            }
            for (int i = nums.size(); i >= 0; --i) {
                for (int j = 0; j < bucket[i].size(); ++j) {
                    res.push_back(bucket[i][j]);
                    if (res.size() == k) return res;
                }
            }
            return res;
        }
    };
    ```