- [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4130577.html)
- [官方解法](https://leetcode-cn.com/problems/single-number/solution/zhi-chu-xian-yi-ci-de-shu-zi-by-leetcode/)
- 解法一：排序
    + 先对数组进行排序，然后每二个元素进行比较即可。
    ```C++
    class Solution {
    public:
        int singleNumber(vector<int>& nums) {
            if(nums.size() == 1)
                return nums[0];
            sort(nums.begin(), nums.end());
            for(int i = 0; i < nums.size() - 1; i += 2)
            {
                if(nums[i] != nums[i + 1])
                    return nums[i];
            }
            return nums[nums.size() - 1]; //    满足条件得元素是最后一个元素时
        }
    };
    ```

- 解法二：哈希集合
    + 题目中让我们在线性的时间复杂度内求解，那么一个非常直接的思路就是使用 HashSet，利用其常数级的查找速度。遍历数组中的每个数字，若当前数字已经在 HashSet 中了，则将 HashSet 中的该数字移除，否则就加入 HashSet。这相当于两两抵消了，最终凡事出现两次的数字都被移除了 HashSet，唯一剩下的那个就是单独数字了，参见代码如下：
    ```C++
    class Solution {
    public:
        int singleNumber(vector<int>& nums) {
            unordered_set<int> s;
            for(int i = 0; i < nums.size(); ++i)
            {
                if(s.count(nums[i]))
                    s.erase(nums[i]);
                else
                    s.insert(nums[i]);
            }
            return *s.begin();
        }
    };
    ```

- 解法三：位运算-异或
    + 题目中让我们不使用额外空间来做，本来是一道非常简单的题，但是由于加上了时间复杂度必须是 O(n)，并且空间复杂度为 O(1)，使得不能用排序方法，也不能使用 HashSet 数据结构。那么只能另辟蹊径，需要用位操作 Bit Operation 来解此题，这个解法如果让我想，肯定想不出来，因为谁会想到用逻辑异或来解题呢。逻辑异或的真值表为：

    ``` 
    异或运算A 异或 B的真值表如下：

        A   B   ⊕
        F   F   F
        F   T   T
        T   F   T
        T   T   F
    ```
    由于数字在计算机是以二进制存储的，每位上都是0或1，如果我们把两个相同的数字异或，0与0 '异或' 是0，1与1 '异或' 也是0，那么我们会得到0。根据这个特点，我们把数组中所有的数字都 '异或' 起来，则每对相同的数字都会得0，然后最后剩下来的数字就是那个只有1次的数字。这个方法确实很赞，但是感觉一般人不会往 '异或' 上想，绝对是为CS专业的同学设计的好题呀，赞一个~~ 
    ```C++
    class Solution {
    public:
        int singleNumber(vector<int>& nums) {
            int res = 0;
            for(auto num : nums)
                res ^= num;
            return res;
        }
    };
    ```
