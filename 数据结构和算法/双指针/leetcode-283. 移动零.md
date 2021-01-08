- [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)
- [参考解法](https://www.cnblogs.com/grandyang/p/4822732.html)
- [官方解法](https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/)
- 自己得解法
    + 从后往前统计，发现有0，就和后面得数字进行交换，知道末尾或者后面的元素是0.
    + 复杂度有点高，可以用双指针进行优化。
    ```C++
    class Solution {
    public:
        void moveZeroes(vector<int>& nums) {
            int right = nums.size() - 1;
            while(right >= 0)
            {
                if(nums[right] == 0)
                {
                    int ind = right + 1;
                    while(ind < nums.size() && nums[ind] != 0)
                    {
                        swap(nums[ind - 1], nums[ind]);
                        ++ind;
                    }
                }
                --right;
            }
        }
    };
    ```
- 双指针
    + 需要用两个指针，一个不停的向后扫，找到非零位置，然后和前面那个指针交换位置即可，参见下面的代码：
    ```C++
    class Solution {
    public:
        void moveZeroes(vector<int>& nums) {
            int l = 0;
            for(int r = 0; r < nums.size(); ++r)
            {
                if(nums[r])
                {
                    swap(nums[l], nums[r]);
                    ++l;
                }
            }
        }
    };
    ```