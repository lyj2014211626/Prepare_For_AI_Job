- [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4233501.html)
- [官方解法](https://leetcode-cn.com/problems/majority-element/solution/duo-shu-yuan-su-by-leetcode-solution/)
- 解法一：哈希表
    + 保存每个元素出现的次数，然后统计找到出现次数最多的元素的下标即可。
    + 时间复杂度和空间复杂度都是O(N)
    ```C++
    class Solution {
    public:
        int majorityElement(vector<int>& nums) {
            unordered_map<int, int> tab;
            int index = 0;
            int res = 0;
            for(int i = 0; i < nums.size(); ++i)
            {
                ++tab[nums[i]];
                if(tab[nums[i]] > res)
                {
                    res = tab[nums[i]];
                    index = i;
                }
            }
            return nums[index];
        }
    };
    ```

- 解法二：摩尔投票法
    - 这是到求大多数的问题，有很多种解法，其中我感觉比较好的有两种，一种是用哈希表，这种方法需要 O(n) 的时间和空间，另一种是用一种叫**摩尔投票法** Moore Voting，需要 O(n) 的时间和 O(1) 的空间，比前一种方法更好。这种投票法先将第一个数字假设为过半数，然后把计数器设为1，比较下一个数和此数是否相等，若相等则计数器加一，反之减一。然后看此时计数器的值，若为零，则将下一个值设为候选过半数。以此类推直到遍历完整个数组，当前候选过半数即为该数组的过半数。不仔细弄懂摩尔投票法的精髓的话，过一阵子还是会忘记的，首先要明确的是这个叼炸天的**方法是有前提的，就是数组中一定要有过半数的存在才能使用**，下面来看本**算法的思路，这是一种先假设候选者，然后再进行验证的算法**。现将数组中的第一个数假设为过半数，然后进行统计其出现的次数，如果遇到同样的数，则计数器自增1，否则计数器自减1，如果计数器减到了0，则更换下一个数字为候选者。这是一个很巧妙的设定，也是本算法的精髓所在，**为啥遇到不同的要计数器减1呢，为啥减到0了又要更换候选者呢**？首先是有那个强大的前提存在，一定会有一个出现超过半数的数字存在，那么如果计数器减到0了话，说明目前不是候选者数字的个数已经跟候选者的出现个数相同了，那么这个候选者已经很 weak，不一定能出现超过半数，此时选择更换当前的候选者。那有可能你会有疑问，那万一后面又大量的出现了之前的候选者怎么办，不需要担心，如果之前的候选者在后面大量出现的话，其又会重新变为候选者，直到最终验证成为正确的过半数，佩服算法的提出者啊，代码如下：
    - 时间复杂度是O(N),空间复杂度是O(1)
    ```C++
    class Solution {
    public:
        int majorityElement(vector<int>& nums) {
            int res = 0;
            int num = 0;
            for(int i = 0; i < nums.size(); ++i)
            {
                if(num == 0)
                {
                    res = nums[i];
                    ++num;
                }
                else
                {
                    res == nums[i] ? ++num : --num;
                }
            }
            return res;
        }
    };
    ```