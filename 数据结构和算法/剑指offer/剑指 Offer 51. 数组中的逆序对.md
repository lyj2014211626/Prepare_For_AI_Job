- [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)
- [参考答案1](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/shu-zu-zhong-de-ni-xu-dui-by-leetcode-solution/)
- [参考答案2](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/solution/bao-li-jie-fa-fen-zhi-si-xiang-shu-zhuang-shu-zu-b/)
- 解法一：归并排序
    + 思想是「分治算法」，所有的「逆序对」来源于 3 个部分
        * 左边区间的逆序对；
        * 右边区间的逆序对；
        * 右边区间的逆序对；
    + 其实主要是计算小于一个数的数目，这样就可以计算出逆序对数。
    ```C++
    class Solution {
    public:
        int mergeSort(vector<int>&nums, vector<int>&vec, int l, int r)
        {
            if( l >= r)
                return 0;
            int mid = (l + r) / 2;
            int res = mergeSort(nums, vec, l, mid) + mergeSort(nums, vec, mid + 1, r);
            int left = l, right = mid + 1, pos = l;
            while(left <= mid && right <= r)
            {
                if(nums[left] <= nums[right])
                {
                    vec[pos] = nums[left];
                    ++left;
                    res += (right - mid - 1);
                }
                else
                {
                    vec[pos] = nums[right];
                    ++right;
                }
                ++pos;
            }
            for(int i = left; i <= mid; ++i)
            {
                vec[pos++] = nums[i];
                res += right - mid - 1;
            }
            for(int i = right; i <= r; ++i)
            {
                vec[pos++] = nums[i];
            }
            copy(vec.begin() + l, vec.begin() + r + 1, nums.begin() + l);
            return res;
        }
        int reversePairs(vector<int>& nums) {
            int n = nums.size();
            vector<int> vec(n,0);
            return mergeSort(nums, vec, 0, n - 1);
        }
    };
    ```