- [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
- [参考博客](https://github.com/grandyang/leetcode/issues/3)
- [官方解法](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetcod/)
- 解法一：滑动窗口
    + 这道求最长无重复子串的题和之前那道 Isomorphic Strings 很类似，属于 LeetCode 早期经典题目，博主认为是可以跟 Two Sum 媲美的一道题。给了我们一个字符串，让求最长的无重复字符的子串，注意这里是子串，不是子序列，所以必须是连续的。先不考虑代码怎么实现，如果给一个例子中的例子 "abcabcbb"，让你手动找无重复字符的子串，该怎么找。博主会一个字符一个字符的遍历，比如 a，b，c，然后又出现了一个a，那么此时就应该去掉第一次出现的a，然后继续往后，又出现了一个b，则应该去掉一次出现的b，以此类推，最终发现最长的长度为3。所以说，需要记录之前出现过的字符，记录的方式有很多，最常见的是统计字符出现的个数，但是这道题字符出现的位置很重要，所以可以使用 HashMap 来建立字符和其出现位置之间的映射。进一步考虑，由于字符会重复出现，到底是保存所有出现的位置呢，还是只记录一个位置？我们之前手动推导的方法实际上是维护了一个滑动窗口，窗口内的都是没有重复的字符，需要尽可能的扩大窗口的大小。由于窗口在不停向右滑动，所以只关心每个字符最后出现的位置，并建立映射。窗口的右边界就是当前遍历到的字符的位置，为了求出窗口的大小，需要一个变量 left 来指向滑动窗口的左边界，这样，如果当前遍历到的字符从未出现过，那么直接扩大右边界，如果之前出现过，那么就分两种情况，在或不在滑动窗口内，如果不在滑动窗口内，那么就没事，当前字符可以加进来，如果在的话，就需要先在滑动窗口内去掉这个已经出现过的字符了，去掉的方法并不需要将左边界 left 一位一位向右遍历查找，由于 HashMap 已经保存了该重复字符最后出现的位置，所以直接移动 left 指针就可以了。维护一个结果 res，每次用出现过的窗口大小来更新结果 res，就可以得到最终结果啦。
    
    + 这里可以建立一个 HashMap，建立每个字符和其最后出现位置之间的映射，然后需要定义两个变量 res 和 left，其中 res 用来记录最长无重复子串的长度，left 指向**该无重复子串左边的起始位置的前一个，由于是前一个，所以初始化就是 -1，**然后遍历整个字符串，对于每一个遍历到的字符，如果该字符已经在 HashMap 中存在了，并且如果其映射值大于 left 的话，那么更新 left 为当前映射值。然后映射值更新为当前坐标i，这样保证了 left 始终为当前边界的前一个位置，然后计算窗口长度的时候，直接用 i-left 即可，用来更新结果 res。
    
    + **这里解释下程序中那个 if 条件语句中的两个条件 m.count(s[i]) && m[s[i]] > left，因为一旦当前字符 s[i] 在 HashMap 已经存在映射，说明当前的字符已经出现过了，而若 m[s[i]] > left 成立，说明之前出现过的字符在窗口内，那么如果要加上当前这个重复的字符，就要移除之前的那个，所以让 left 赋值为 m[s[i]]**，由于 left 是窗口左边界的前一个位置（这也是 left 初始化为 -1 的原因，因为窗口左边界是从0开始遍历的），所以相当于已经移除出滑动窗口了。举一个最简单的例子 "aa"，当 i=0 时，建立了 a->0 的映射，并且此时结果 res 更新为1，那么当 i=1 的时候，发现a在 HashMap 中，并且映射值0大于 left 的 -1，所以此时 left 更新为0，映射对更新为 a->1，那么此时 i-left 还为1，不用更新结果 res，那么最终结果 res 还为1，正确，代码如下：
    + C++版本：这是我在腾讯暑期实习生二面的题目，我这里划分了二种情况，分别是找到重复的字符和没有找到重复的字符。
    ```
    class Solution {
    public:
        int lengthOfLongestSubstring(string s) {
            if(s.empty())
                return 0;
            unordered_map<char, int> map_tab;
            int max_size = 1;
            int left = -1;//这里只能是-1
            for(int i = 0; i < s.size(); ++i)
            {
                if(map_tab.find(s[i]) != map_tab.end() && map_tab[s[i]] >= left)//后面判单索引要在left之后
                {
                    max_size = max(max_size, i - map_tab[s[i]]);//当前发生重复字符的长度
                    left = map_tab[s[i]];//记录非重复字符串的左索引 不能加1 因为计算长度的时候没有加1
                    map_tab[s[i]] = i;//更新重复字符的索引
                }
                else
                {
                    max_size = max(max_size, i - left);
                    map_tab[s[i]] = i;
                }
            }
            return max_size;
        }
    };
    ```
    
    + C++精简版
    ```
    class Solution {
    public:
        int lengthOfLongestSubstring(string s) {
            int res = 0, left = -1, n = s.size();
            unordered_map<int, int> m;
            for (int i = 0; i < n; ++i) {
                if (m.count(s[i]) && m[s[i]] > left) {
                    left = m[s[i]];  
                }
                m[s[i]] = i;//这二句话在满足条件和不满足条件的时候都是要做的
                res = max(res, i - left);            
            }
            return res;
        }
    };
    ```

    + 下面这种写法是上面解法的精简模式，这里我们可以建立一个 256 位大小的整型数组来代替 HashMap，这样做的原因是 ASCII 表共能表示 256 个字符，但是由于键盘只能表示 128 个字符，所以用 128 也行，然后全部初始化为 -1，这样的好处是不用像之前的 HashMap 一样要查找当前字符是否存在映射对了，对于每一个遍历到的字符，直接用其在数组中的值来更新 left，因为默认是 -1，而 left 初始化也是 -1，所以并不会产生错误，这样就省了 if 判断的步骤，其余思路都一样：
    + 在线提交之后，提却提升不少的性能
    ```
    class Solution {
    public:
        int lengthOfLongestSubstring(string s) {
            vector<int> m(128, -1);
            int res = 0, left = -1;
            for (int i = 0; i < s.size(); ++i) {
                left = max(left, m[s[i]]);
                m[s[i]] = i;
                res = max(res, i - left);
            }
            return res;
        }
    };
    ```