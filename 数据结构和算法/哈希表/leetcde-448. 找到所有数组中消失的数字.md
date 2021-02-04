- [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)
- [参考链接](https://www.cnblogs.com/grandyang/p/6222149.html)
- 解法一：索引
    + 我们知道，这种不需要额外的空间的算法都需要索引来作为数值；这里有一个技巧，就是在发生修改的数组中，直接将其置为相反数，下次继续访问的时候直接取绝对值就好。
    + C++版本
```C++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        for(int i = 0; i < nums.size(); ++i){
            int idx = abs(nums[i]) - 1;
            nums[idx] = nums[idx] > 0 ? -nums[idx] : nums[idx];
        }
        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] > 0){
                res.push_back(i + 1);
            }
        }
        return res;
    }
};
```
    
    + pyhon版本
```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        res = []
        for ind in range(0, len(nums)):
            new_index = abs(nums[ind]) - 1
            if nums[new_index] > 0:
                nums[new_index] = -nums[new_index]
        for ind in range(0, len(nums)):
            if nums[ind] > 0:
                res.append(ind + 1)
        return res
```
