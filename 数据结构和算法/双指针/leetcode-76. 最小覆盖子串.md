- [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
- [参考博客](https://www.cnblogs.com/grandyang/p/4340948.html)
- [官方解法](https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-2/)
- 解法一： 哈希表+滑动窗口
    + 这道题给了我们一个原字符串S，还有一个目标字符串T，让在S中找到一个最短的子串，使得其包含了T中的所有的字母，并且限制了时间复杂度为 O(n)。这道题的要求是要在 O(n) 的时间度里实现找到这个最小窗口字串，暴力搜索 Brute Force 肯定是不能用的，因为遍历所有的子串的时间复杂度是平方级的。那么来想一下，时间复杂度卡的这么严，说明必须在一次遍历中完成任务，当然遍历若干次也是 O(n)，但不一定有这个必要，尝试就一次遍历拿下！那么再来想，既然要包含T中所有的字母，那么对于T中的每个字母，肯定要快速查找是否在子串中，既然总时间都卡在了 O(n)，肯定不想在这里还浪费时间，就用空间换时间（也就算法题中可以这么干了，七老八十的富翁就算用大别野也换不来时间啊。依依东望，望的就是时间呐 T.T），使用 **HashMap，建立T中每个字母与其出现次数之间的映射**，那么你可能会有疑问，为啥不用 HashSet 呢，别急，讲到后面你就知道用 HashMap 有多妙，简直妙不可言～
    + 目前在脑子一片浆糊的情况下，我们还是从简单的例子来分析吧，题目例子中的S有点长，换个短的 S = "ADBANC"，T = "ABC"，那么肉眼遍历一遍S呗，首先第一个是A，嗯很好，T中有，第二个是D，T中没有，不理它，第三个是B，嗯很好，T中有，第四个又是A，多了一个，礼多人不怪嘛，收下啦，第五个是N，一边凉快去，第六个终于是C了，那么貌似好像需要整个S串，其实不然，注意之前有多一个A，就算去掉第一个A，也没事，因为第四个A可以代替之，第二个D也可以去掉，因为不在T串中，第三个B就不能再去掉了，不然就没有B了。所以最终的答案就"BANC"了。通过上面的描述，你有没有发现一个有趣的现象，先扩展，再收缩，就好像一个窗口一样，先扩大右边界，然后再收缩左边界，上面的例子中右边界无法扩大了后才开始收缩左边界，实际上对于复杂的例子，有可能是扩大右边界，然后缩小一下左边界，然后再扩大右边界等等。这就很像一个不停滑动的窗口了，**这就是大名鼎鼎的滑动窗口 Sliding Window 了，简直是神器啊，能解很多子串，子数组，子序列等等的问题，**是必须要熟练掌握的啊！
    
    + 下面来考虑用代码来实现，先来回答一下前面埋下的伏笔，为啥要用 HashMap，而不是 HashSet，现在应该很显而易见了吧，因为要统计T串中字母的个数，而不是仅仅看某个字母是否在T串中出现。统计好T串中字母的个数了之后，开始遍历S串，对于S中的每个遍历到的字母，都在 HashMap 中的映射值减1，如果减1后的映射值仍大于等于0，说明当前遍历到的字母是T串中的字母，使用一个计数器 cnt，使其自增1。当 cnt 和T串字母个数相等时，说明此时的窗口已经包含了T串中的所有字母，此时更新一个 minLen 和结果 res，这里的 minLen 是一个全局变量，用来记录出现过的包含T串所有字母的最短的子串的长度，结果 res 就是这个最短的子串。然后开始收缩左边界，由于遍历的时候，对映射值减了1，所以此时去除字母的时候，就要把减去的1加回来，此时如果加1后的值大于0了，说明此时少了一个T中的字母，那么 cnt 值就要减1了，然后移动左边界 left。你可能会疑问，对于不在T串中的字母的映射值也这么加呀减呀的，真的大丈夫（带胶布）吗？其实没啥事，因为对于不在T串中的字母，减1后，变-1，cnt 不会增加，之后收缩左边界的时候，映射值加1后为0，cnt 也不会减少，所以并没有什么影响啦，下面是具体的步骤啦：
    + 先扫描一遍T，把对应的字符及其出现的次数存到 HashMap 中。
    + 然后开始遍历S，就把遍历到的字母对应的 HashMap 中的 value 减一，如果减1后仍大于等于0，cnt 自增1。
    + 如果 cnt 等于T串长度时，开始循环，纪录一个字串并更新最小字串值。然后将子窗口的左边界向右移，如果某个移除掉的字母是T串中不可缺少的字母，那么 cnt 自减1，表示此时T串并没有完全匹配。
    ```C++
    class Solution {
    public:
        string minWindow(string s, string t) {
            string res = "";
            unordered_map<char, int> letterCnt;
            int left = 0, cnt = 0, minLen = INT_MAX;
            for (char c : t) ++letterCnt[c];
            for (int i = 0; i < s.size(); ++i) {
                if (--letterCnt[s[i]] >= 0) ++cnt;
                while (cnt == t.size()) {
                    if (minLen > i - left + 1) {
                        minLen = i - left + 1;
                        res = s.substr(left, minLen);
                    }
                    if (++letterCnt[s[left]] > 0) --cnt;
                    ++left;
                }
            }
            return res;
        }
    };
    ```

    + 性能提升
    ```C++
    class Solution {
    public:
        string minWindow(string s, string t) {
            vector<int> letterCnt(128, 0);
            int left = 0, cnt = 0, minLeft = -1, minLen = INT_MAX;
            for (char c : t) ++letterCnt[c];
            for (int i = 0; i < s.size(); ++i) {
                if (--letterCnt[s[i]] >= 0) ++cnt;
                while (cnt == t.size()) {
                    if (minLen > i - left + 1) {
                        minLen = i - left + 1;
                        minLeft = left;
                    }
                    if (++letterCnt[s[left]] > 0) --cnt;
                    ++left;
                }
            }
            return minLeft == -1 ? "" : s.substr(minLeft, minLen);
        }
    };
    ```