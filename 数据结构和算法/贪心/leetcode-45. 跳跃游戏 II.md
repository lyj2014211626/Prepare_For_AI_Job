- [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)
- [参考博客](https://github.com/grandyang/leetcode/issues/45)
- 解法一：贪心算法
    + 这题是之前那道 Jump Game 的延伸，那题是问能不能到达最后一个数字，而此题只让求到达最后一个位置的最少跳跃数，貌似是默认一定能到达最后位置的? 此题的核心方法是利用**贪婪算法** Greedy 的思想来解，想想为什么呢？ 为了较快的跳到末尾，想知道每一步能跳的范围，这里贪婪并不是要在能跳的范围中选跳力最远的那个位置，因为这样选下来不一定是最优解，这么一说感觉又有点不像贪婪算法了。其实这里贪的是一个能到达的最远范围，遍历当前跳跃能到的所有位置，然后根据该位置上的跳力来预测下一步能跳到的最远距离，贪出一个最远的范围，一旦当这个范围到达末尾时，当前所用的步数一定是最小步数。需要两个变量 cur 和 pre 分别来保存当前的能到达的最远位置和之前能到达的最远位置，只要 cur 未达到最后一个位置则循环继续，pre 先赋值为 cur 的值，表示上一次循环后能到达的最远位置，如果当前位置i小于等于 pre，说明还是在上一跳能到达的范围内，根据当前位置加跳力来更新 cur，更新 cur 的方法是比较当前的 cur 和 i + A[i] 之中的较大值，如果题目中未说明是否能到达末尾，还可以判断此时 pre 和 cur 是否相等，如果相等说明 cur 没有更新，即无法到达末尾位置，返回 -1，代码如下：
    ```
    class Solution {
    public:
        int jump(vector<int>& nums) {
            int n = nums.size();
            int res = 0, cur = 0, i = 0;
            while(cur < n - 1){
                ++res;
                int pre = cur;//保存上一次到达的最大位置
                for(; i <= pre; ++i){//选择下一跳到达的最远位置
                    cur = max(cur, nums[i] + i);
                }
            }
            return res;
        }
    };
    ```

- 解法二
    + 还有一种写法，跟上面那解法略有不同，但是本质的思想还是一样的，关于此解法的详细分析可参见网友 [实验室小纸贴校外版的博客](https://www.cnblogs.com/lichen782/p/leetcode_Jump_Game_II.html)，这里 cur 是当前能到达的最远位置，last 是上一步能到达的最远位置，遍历数组，首先用 i + nums[i] 更新 cur，这个在上面解法中讲过了，然后判断如果当前位置到达了 last，即上一步能到达的最远位置，说明需要再跳一次了，将 last 赋值为 cur，并且步数 res 自增1，这里小优化一下，判断如果 cur 到达末尾了，直接 break 掉即可，代码如下：
    ```
    class Solution {
    public:
        int jump(vector<int>& nums) {
            int n = nums.size();
            int res = 0, cur = 0, last = 0;
            for(int i = 0; i < n- 1; ++i){
                cur = max(cur, nums[i] + i);
                if(i == last){
                    last = cur;
                    ++res;
                    if(cur >= n - 1)
                        break;
                }
            }
            return res;
        }
    };
    ```