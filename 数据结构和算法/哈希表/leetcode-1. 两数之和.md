- [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
- [参考博客](https://github.com/grandyang/leetcode/issues/1)
- [官方解法](https://leetcode-cn.com/problems/two-sum/solution/liang-shu-zhi-he-by-leetcode-2/)
- 解法一：哈希表
    + 这道题给了我们一个数组，还有一个目标数target，让找到两个数字，使其和为 target，乍一看就感觉可以用暴力搜索，但是猜到 OJ 肯定不会允许用暴力搜索这么简单的方法，于是去试了一下，果然是 Time Limit Exceeded，这个算法的时间复杂度是 O(n^2)。那么只能想个 O(n) 的算法来实现，由于暴力搜索的方法是遍历所有的两个数字的组合，然后算其和，这样虽然节省了空间，但是时间复杂度高。一般来说，为了提高时间的复杂度，需要用空间来换，这算是一个 trade off 吧，但这里只想用线性的时间复杂度来解决问题，就是说只能遍历一个数字，那么另一个数字呢，可以事先将其存储起来，使用一个 HashMap，来建立数字和其坐标位置之间的映射，由于 HashMap 是常数级的查找效率，这样在遍历数组的时候，用 target 减去遍历到的数字，就是另一个需要的数字了，直接在 HashMap 中查找其是否存在即可，注意要判断查找到的数字不是第一个数字，比如 target 是4，遍历到了一个2，那么另外一个2不能是之前那个2，整个实现步骤为：先遍历一遍数组，建立 HashMap 映射，然后再遍历一遍，开始查找，找到则记录 index。代码如下：
    + C++版本
    ```
    class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            unordered_map<int, int> tab ;
            vector<int> res;
            for(int i = 0; i < nums.size(); ++i)
            {
                tab[nums[i]] = i;
            }
            for(int i = 0; i < nums.size(); ++i)
            {
                int sub = target - nums[i];
                if(tab.find(sub) != tab.end() && tab[sub] != i)//这里后面的一个条件很重要 容易丢失
                {
                    res.push_back(i);
                    res.push_back(tab[sub]);
                    break;
                }
            }
            return res;
        }
    };
    ```
    
    + 简化版 在条件的判断少一个
    ```
    class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            unordered_map<int, int> m;
            for (int i = 0; i < nums.size(); ++i) {
                if (m.count(target - nums[i])) {
                    return {i, m[target - nums[i]]};
                }
                m[nums[i]] = i;
            }
            return {};
        }
    };
    ```