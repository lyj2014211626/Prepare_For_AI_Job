- [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)
- [参考博客](https://github.com/grandyang/leetcode/issues/6)
- [官方解答](https://leetcode-cn.com/problems/zigzag-conversion/solution/z-zi-xing-bian-huan-by-leetcode/)
- 解法一：找规律
    + 可以发现，除了第一行和最后一行没有中间形成之字型的数字外，其他都有，而首位两行中相邻两个元素的 index 之差跟行数是相关的，为  2*nRows - 2, 根据这个特点，可以按顺序找到所有的黑色元素在元字符串的位置，将他们按顺序加到新字符串里面。对于红色元素出现的位置（Github 上可能无法正常显示颜色，[请参见博客园上的帖子](https://www.cnblogs.com/grandyang/p/4128268.html)）也是有规律的，每个红色元素的位置为 j + 2 x numRows-2 - 2 x i, 其中，j为前一个黑色元素的列数，i为当前行数。 比如当 n = 4 中的那个红色5，它的位置为 1 + 2 x 4-2 - 2 x 1 = 5，为原字符串的正确位置。知道了所有黑色元素和红色元素位置的正确算法，就可以一次性的把它们按顺序都加到新的字符串里面。代码如下：
    + C++版本
    ```
    class Solution {
    public:
        string convert(string s, int numRows) {
            if(numRows <= 1)    //注意边界条件的判断
                return s;
            string res;
            int size = 2 * numRows - 2;//一个规律的循环
            int n = s.size();
            for(int row = 0; row < numRows; ++row)
            {
                for(int j = row; j < n; j += 2 * numRows - 2)
                {
                    res += s[j];
                    int pos = j + size - 2 * row;
                    if(row != 0 && row != numRows - 1 && pos < n )
                        res += s[pos];
                }
            }
            return res;
        }
    };
    ```

- 解法二
    + 若上面解法中的规律不是很好想的话，我们也可以用下面这种更直接的方法来做，建立一个大小为 numRows 的字符串数组，为的就是把之字形的数组整个存进去，然后再把每一行的字符拼接起来，就是想要的结果了。顺序就是按列进行遍历，首先前 numRows 个字符就是按顺序存在每行的第一个位置，然后就是 ‘之’ 字形的连接位置了，可以发现其实都是在行数区间 [1, numRows-2] 内，只要按顺序去取字符就可以了，最后把每行都拼接起来即为所求，参见代码如下：
    + C++版本
    ```
    class Solution {
    public:
        string convert(string s, int numRows) {
            if (numRows <= 1) return s;
            string res;
            int i = 0, n = s.size();
            vector<string> vec(numRows);
            while (i < n) {
                for (int pos = 0; pos < numRows && i < n; ++pos) {
                    vec[pos] += s[i++];
                }
                for (int pos = numRows - 2; pos >= 1 && i < n; --pos) {
                    vec[pos] += s[i++];
                }
            }
            for (auto &a : vec) res += a;
            return res;
        }
    };
    ```