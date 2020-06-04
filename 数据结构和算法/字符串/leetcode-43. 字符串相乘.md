- [43. 字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)
- [参考博客](https://github.com/grandyang/leetcode/issues/43)
- [官方解法](https://leetcode-cn.com/problems/multiply-strings/solution/you-hua-ban-shu-shi-da-bai-994-by-breezean/)
- 解法一： 普通竖式
    + 按照普通的乘法计算规则来实现即可，后面的乘法结果要填0.
    + C++ 比较普通冗余的实现方式
    ```
    class Solution {
    public:
        string multiply(string num1, string num2) {
            int n1 = num1.size();
            int n2 = num2.size();
            string res = "0";
            if(num1 == "0" || num2 == "0")//边界条件判断
                return "0";
            for(int i = n2 - 1; i >=0; --i)
            {
                string t = "";
                int carry = 0;
                for(int c = 0; c < (n2 - 1 - i); ++c)//末尾添0
                    t += '0';
                
                for(int j = n1 - 1; j >=0; --j)//依次计算竖式值
                {
                    t += (carry + (num1[j] - '0') * (num2[i] - '0')) % 10 + '0';
                    carry = (carry + (num1[j] - '0') * (num2[i] - '0')) / 10;
                }
                if(carry)//考虑进位
                {
                    t += carry + '0';
                }
          
                //字符串求和
                string tt = "";
                int ind1 = 0;
                int ind2 = 0;
                int res_carry = 0;
                while(ind1 < res.size() || ind2 <t.size())
                {
                    int left = ind1 < res.size() ? (res[ind1] - '0') : 0;
                    int right = ind2 < t.size() ? (t[ind2] - '0') : 0;
                    tt += (res_carry + left + right) % 10 + '0';
                    res_carry = (res_carry + left + right) / 10;
                    ++ind1;
                    ++ind2;
                }
     
                if(res_carry)
                    tt += res_carry + '0';
                res = tt;

            }
            
            reverse(res.begin(), res.end());//字符串反转
            return res;
        }
    };
    ```
    + 这道题让我们求两个字符串数字的相乘，输入的两个数和返回的数都是以字符串格式储存的，这样做的原因可能是这样可以计算超大数相乘，可以不受 int 或 long 的数值范围的约束，那么该如何来计算乘法呢，小时候都学过多位数的乘法过程，都是**每位相乘然后错位相加**，那么这里就是用到这种方法，举个例子，比如 89 x 76，那么根据小学的算术知识，不难写出计算过程如下：
    ```      
            8 9  <- num2
            7 6  <- num1
        -------
            5 4
          4 8
          6 3
        5 6
        -------
        6 7 6 4
    ```
    + 如果自己再写些例子出来，不难发现，**两数相乘得到的乘积的长度其实其实不会超过两个数字的长度之和**，若 num1 长度为m，num2 长度为n，则 num1 x num2 的长度不会超过 m+n，还有就是要明白乘的时候为什么要错位，比如6乘8得到的 48 为啥要跟6乘9得到的 54 错位相加，因为8是十位上的数字，其本身相当于80，所以错开的一位实际上末尾需要补的0。还有一点需要观察出来的就是，num1 和 num2 中任意位置的两个数字相乘，得到的两位数在最终结果中的**位置是确定的**，**比如 num1 中位置为i的数字乘以 num2 中位置为j的数字，那么得到的两位数字的位置为 i+j 和 i+j+1**，明白了这些后，就可以进行错位相加了，累加出最终的结果。

    ```
    class Solution {
    public:
        string multiply(string num1, string num2) {
            int n1 = num1.size();
            int n2 = num2.size();
            string res = "";
            vector<int> t(n1 + n2, 0);
            for(int i = n1 - 1; i >= 0; --i)
            {
                for(int j = n2 - 1; j >=0; --j)
                {
                    int mul = (num1[i] - '0') * (num2[j] - '0');
                    int p1 = i + j;
                    int p2 = i + j + 1;
                    int sum = t[p2] + mul;//低位进行连加
                    t[p2] = sum % 10;//地位取余  但是不+=
                    t[p1] += sum / 10;//高位进位  要+=
                }
            }
            for(int num : t)
                if(!res.empty() || num != 0)//边界条件的判断
                    res.push_back(num + '0');
            return res.empty() ? "0" : res;//边界条件的判断
        }
    };
    ```
