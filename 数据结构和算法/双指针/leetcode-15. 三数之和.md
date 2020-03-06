- [15. 三数之和](https://leetcode-cn.com/problems/3sum/)
- [解答博客](https://github.com/grandyang/leetcode/issues/15)
- [官方解法](https://leetcode-cn.com/problems/3sum/solution/pai-xu-shuang-zhi-zhen-zhu-xing-jie-shi-python3-by/)
- 解法：排序+双指针
    + 这道题让我们求三数之和，比之前那道 Two Sum 要复杂一些，博主考虑过先 fix 一个数，然后另外两个数使用 Two Sum 那种 HashMap 的解法，但是会有重复结果出现，就算使用 TreeSet 来去除重复也不行，会 TLE，看来**此题并不是考 Two Sum 的解法**。来分析一下这道题的特点，要找出三个数且和为0，那么除了三个数全是0的情况之外，肯定会有负数和正数，还是要先 fix 一个数，然后去找另外两个数，只要找到两个数且和为第一个 fix 数的相反数就行了，既然另外两个数不能使用 Two Sum 的那种解法来找，如何能更有效的定位呢？**我们肯定不希望遍历所有两个数的组合吧，所以如果数组是有序的，那么就可以用双指针以线性时间复杂度来遍历所有满足题意的两个数组合。**
    + 对原数组进行排序，然后开始遍历排序后的数组，这里注意不是遍历到最后一个停止，而是到倒数第三个就可以了。这里可以先做个剪枝优化，就是当遍历到正数的时候就 break，为啥呢，因为数组现在是有序的了，如果第一个要 fix 的数就是正数了，则后面的数字就都是正数，就永远不会出现和为0的情况了。然后还要加上重复就跳过的处理，处理方法是从第二个数开始，如果和前面的数字相等，就跳过，因为不想把相同的数字fix两次。对于遍历到的数，用0减去这个 fix 的数得到一个 target，然后只需要再之后找到两个数之和等于 target 即可。用两个指针分别指向 fix 数字之后开始的数组首尾两个数，如果两个数和正好为 target，则将这两个数和 fix 的数一起存入结果中。然后就是跳过重复数字的步骤了，两个指针都需要检测重复数字。如果两数之和小于 target，则将左边那个指针i右移一位，使得指向的数字增大一些。同理，如果两数之和大于 target，则将右边那个指针j左移一位，使得指向的数字减小一些，代码如下：
    + C++版本
    ```
    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) {
            vector<vector<int>> res;
            sort(nums.begin(), nums.end());//排序
            if(nums.size() < 3 || nums.front() > 0 || nums.back() < 0)//临界条件判断
                return {};
            for(int i = 0; i < nums.size() - 2; ++i)
            {
                if(nums[i] > 0)//后面的所有组合均不符合情况
                    break;
                if(i > 0 && nums[i] == nums[i - 1])//剪枝 重复直接跳出
                    continue;
                int left = i + 1;
                int right = nums.size() - 1;
                int target = 0 - nums[i];
                while(left < right)
                {
                    if((nums[left] + nums[right]) == target)
                    {
                        res.push_back({nums[i], nums[left], nums[right]});
                        
                        while(left < right && nums[left] == nums[left + 1])// 跳过重复项
                            ++left;
                        while(left < right && nums[right] == nums[right - 1])
                            --right;
                        
                        ++left;//进行一个组合的索引
                        --right;
                    }
                    else if(nums[left] + nums[right] < target)//不相等
                        ++left;
                    else
                        --right;
                }
            }
            return res;
        }
    };
    ```